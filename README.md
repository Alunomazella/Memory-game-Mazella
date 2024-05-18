# Memory-game-Mazella

index.html:

<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Jogo da Memória</title>
    <link rel="stylesheet" href="styles.css">
</head>
<body>
    <div id="game-board"></div>
    <script src="game.js"></script>
</body>
</html>

styles.css:

body {
    font-family: Arial, sans-serif;
    display: flex;
    justify-content: center;
    align-items: center;
    height: 100vh;
    background-color: #f0f0f0;
    margin: 0;
}

#game-board {
    display: grid;
    grid-template-columns: repeat(4, 100px);
    gap: 10px;
    background-color: #fff;
    padding: 10px;
    border: 1px solid #ccc;
    box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
}

.card {
    width: 100px;
    height: 100px;
    background-color: #007bff;
    display: flex;
    justify-content: center;
    align-items: center;
    font-size: 24px;
    color: white;
    cursor: pointer;
}

.card.flipped {
    background-color: #fff;
    color: #007bff;
    cursor: default;
}

game.js:

document.addEventListener('DOMContentLoaded', () => {
    const gameBoard = document.getElementById('game-board');
    const cards = [
        'A', 'A', 'B', 'B', 'C', 'C', 'D', 'D',
        'E', 'E', 'F', 'F', 'G', 'G', 'H', 'H'
    ];
    let firstCard = null;
    let secondCard = null;
    let lockBoard = false;

    // Função para embaralhar as cartas
    function shuffle(array) {
        array.sort(() => Math.random() - 0.5);
    }

    // Função para criar o tabuleiro do jogo
    function createBoard() {
        shuffle(cards);
        cards.forEach(card => {
            const cardElement = document.createElement('div');
            cardElement.classList.add('card');
            cardElement.dataset.card = card;
            cardElement.innerHTML = '?';
            cardElement.addEventListener('click', flipCard);
            gameBoard.appendChild(cardElement);
        });
    }

    // Função para virar a carta
    function flipCard() {
        if (lockBoard) return;
        if (this === firstCard) return;

        this.classList.add('flipped');
        this.innerHTML = this.dataset.card;

        if (!firstCard) {
            firstCard = this;
        } else {
            secondCard = this;
            checkForMatch();
        }
    }

    // Função para verificar se há correspondência entre as cartas
    function checkForMatch() {
        let isMatch = firstCard.dataset.card === secondCard.dataset.card;
        isMatch ? disableCards() : unflipCards();
    }

    // Função para desabilitar cartas após correspondência
    function disableCards() {
        firstCard.removeEventListener('click', flipCard);
        secondCard.removeEventListener('click', flipCard);
        resetBoard();
    }

    // Função para desvirar as cartas se não houver correspondência
    function unflipCards() {
        lockBoard = true;
        setTimeout(() => {
            firstCard.classList.remove('flipped');
            secondCard.classList.remove('flipped');
            firstCard.innerHTML = '?';
            secondCard.innerHTML = '?';
            resetBoard();
        }, 1500);
    }

    // Função para resetar o tabuleiro
    function resetBoard() {
        [firstCard, secondCard, lockBoard] = [null, null, false];
    }

    createBoard();
});
