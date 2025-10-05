# BirdGame (Clone estilo Flappy Bird)

Projeto Unity (versão **2021.3.5f1**) criado como jogo simples de reflexo/arcade para demonstrar conceitos básicos de:

- Física 2D (Rigidbody2D, colisões, triggers)
- Spawning procedural de obstáculos
- UI (pontuação e tela de Game Over)
- Organização de scripts e uso de Tags / Layers

---

## 🎮 Visão Geral

O jogador controla um pássaro que sobe ao pressionar a tecla Espaço. Tubos aparecem em alturas aleatórias e se movem pela tela. Ao atravessar o espaço central entre os tubos o jogador ganha pontos. Qualquer colisão com um tubo ou com o chão finaliza a partida.

---

## ✨ Funcionalidades Principais

- Mecânica de pulo (flap) responsiva
- Geração infinita de pares de tubos com variação vertical
- Contagem de pontuação ao passar pelo meio dos tubos
- Tela de Game Over com opção de reinício
- Código simples e extensível para experimentação

---

## 🗂️ Estrutura Relevante do Projeto

```
Assets/
  Scenes/
    GameScreen.unity        <- Cena principal
  Scripts/
    BirdScript.cs           <- Controle do pássaro
    LogicScript.cs          <- Pontuação e Game Over
    PipeSpawnScript.cs      <- Spawner de tubos
    PipeMoveScript.cs       <- Movimento/reciclagem de tubos
    PipeMiddleScript.cs     <- Trigger de pontuação
  Sprites/                  <- Artes 2D (pássaro, nuvem, tubo)
TextMesh Pro/               <- Fonte e assets de UI
ProjectSettings/            <- Configurações Unity
```

---

## 📜 Descrição dos Scripts

### `BirdScript.cs`

Controla o movimento vertical do pássaro. Ao pressionar `Space` aplica uma velocidade vertical usando `flapStrength`. Em qualquer colisão chama `logic.gameOver()` e desativa controles (`birdIsAlive = false`).

### `LogicScript.cs`

Gerencia a pontuação e a UI:

- `addScore(int)` atualiza `playerScore` e o texto na tela
- `gameOver()` ativa o painel de Game Over
- `restartGame()` recarrega a cena atual

### `PipeSpawnScript.cs`

Cria instâncias do prefab de tubo em intervalo fixo (`spawnRate`) com variação vertical aleatória dentro de `±heightOffset` a partir da posição do spawner.

### `PipeMoveScript.cs`

Responsável por destruir tubos quando saem da área útil. (OBS: a lógica atual destrói quando `transform.position.x > deadZone`. Caso os tubos se movam para a esquerda a partir de X positivo, normalmente o correto seria destruir quando `x < deadZone`. Ver seção "Possíveis Melhorias").

### `PipeMiddleScript.cs`

Colocado em um collider invisível entre os tubos superior e inferior. Quando o pássaro (Layer 3 no código atual) entra no trigger, chama `logic.addScore(1)`.

---

## 🏷️ Tags e Layers Importantes

| Elemento                 | Requisito                                                                          |
| ------------------------ | ---------------------------------------------------------------------------------- |
| Objeto com `LogicScript` | Tag: `Logic` (necessário para `FindGameObjectWithTag`)                             |
| Pássaro                  | Layer configurada para índice `3` (ou adaptar a verificação em `PipeMiddleScript`) |
| Collider de pontuação    | Deve ser `IsTrigger` = true                                                        |

Se preferir não depender do índice de layer, substitua a checagem em `PipeMiddleScript` por comparação de Tag:

```csharp
if (collision.CompareTag("Player")) { logic.addScore(1); }
```

---

## ⚙️ Parâmetros Ajustáveis

| Script          | Campo                                    | Função                              | Dica de Ajuste                                    |
| --------------- | ---------------------------------------- | ----------------------------------- | ------------------------------------------------- |
| BirdScript      | flapStrength                             | Intensidade do pulo                 | Aumente para um jogo mais "arcade"                |
| PipeSpawnScript | spawnRate                                | Intervalo entre tubos               | Valores menores = mais difícil                    |
| PipeSpawnScript | heightOffset                             | Variação máxima de altura           | Maior => distribuição mais ampla                  |
| PipeMoveScript  | moveSpeed (não aplicado no código atual) | Velocidade dos tubos                | Pode ser usado se adicionar movimento direto aqui |
| PipeMoveScript  | deadZone                                 | Posição X onde tubos são destruídos | Ajuste conforme a largura da cena                 |

---

## ▶️ Como Executar

1. Instale / abra o Unity **2021.3.5f1** (ou versão LTS 2021.x compatível).
2. Abra a pasta raiz do projeto (`BirdGame-Unity`).
3. Carregue a cena `GameScreen.unity` (pasta `Assets/Scenes`).
4. Verifique se o objeto de lógica tem tag `Logic` e se o pássaro tem o Rigidbody2D referenciado em `BirdScript`.
5. Pressione Play.
6. Use a tecla `Espaço` para fazer o pássaro subir.

---

## 🧪 Testes Manuais Rápidos

| Caso      | Ação                                   | Resultado Esperado             |
| --------- | -------------------------------------- | ------------------------------ |
| Pulo      | Pressionar Space                       | Pássaro ganha impulso vertical |
| Colisão   | Bater no tubo                          | Tela de Game Over ativa        |
| Pontuação | Passar entre tubos                     | Pontuação +1 atualizada na UI  |
| Reinício  | Clicar botão (ou chamar `restartGame`) | Cena recarregada com score 0   |

---

## 📦 Dependências

Somente pacotes padrão do Unity + TextMesh Pro (já incluso no projeto). Nenhuma dependência externa adicional.

---

## 🔄 Fluxo de Jogo Simplificado

1. Cena inicia -> Spawner cria primeiro tubo
2. Jogador pressiona Espaço -> pássaro sobe
3. Tubos passam -> trigger central soma ponto
4. Colisão -> Game Over -> jogador pode reiniciar

---

## Vídeos!



https://github.com/user-attachments/assets/22301298-543c-456d-b3e0-4a89b7484dcc




https://github.com/user-attachments/assets/a8042867-8397-4b8c-9ec9-4ce1f1e528aa



