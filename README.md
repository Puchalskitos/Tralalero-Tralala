-----

```html
<!DOCTYPE html>
<html lang="pt-br">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Flashcards 2º H - Temas Diversos</title>
    <link rel="stylesheet" href="style.css">
</head>
<body>
    <header class="main-header">
        <h1>Flashcards Interativos - 2º H</h1>
        <p>Desenvolvido por: Luiz Felipe, Diego Henrique e Vinícius Ferreira</p>
    </header>

    <main class="flashcard-container">
        </main>

    <footer class="main-footer">
        <p>&copy; 2025 - Trabalho de Escola - Turma 2º H</p>
    </footer>

    <script src="script.js"></script>
</body>
</html>
```

-----

```css
:root {
    /* Variáveis de Cores */
    --primary-color: #4CAF50; /* Verde */
    --secondary-color: #FFC107; /* Amarelo */
    --background-color: #f4f4f4;
    --text-color: #333;
    --card-bg-color: #ffffff;
    --card-border-color: #ddd;
    --shadow-color: rgba(0, 0, 0, 0.1);

    /* Variáveis de Espaçamento */
    --padding-sm: 10px;
    --padding-md: 20px;
    --margin-sm: 10px;
    --margin-md: 20px;

    /* Outras Variáveis */
    --border-radius: 8px;
    --transition-speed: 0.3s;
}

body {
    font-family: 'Arial', sans-serif;
    margin: 0;
    padding: 0;
    background-color: var(--background-color);
    color: var(--text-color);
    display: flex;
    flex-direction: column;
    min-height: 100vh;
}

.main-header {
    background-color: var(--primary-color);
    color: white;
    padding: var(--padding-md);
    text-align: center;
    box-shadow: 0 2px 4px var(--shadow-color);
}

.main-header h1 {
    margin: 0;
    font-size: 2em;
}

.main-header p {
    margin-top: var(--margin-sm);
    font-size: 1.1em;
}

.flashcard-container {
    display: flex;
    flex-wrap: wrap;
    justify-content: center;
    gap: var(--margin-md);
    padding: var(--padding-md);
    flex-grow: 1; /* Garante que ocupe o espaço restante */
}

.flashcard {
    background-color: var(--card-bg-color);
    border: 1px solid var(--card-border-color);
    border-radius: var(--border-radius);
    width: 300px;
    height: 200px;
    display: flex;
    align-items: center;
    justify-content: center;
    text-align: center;
    cursor: pointer;
    box-shadow: 0 4px 8px var(--shadow-color);
    perspective: 1000px; /* Necessário para a rotação 3D */
    transition: transform var(--transition-speed) ease;
}

.flashcard-inner {
    position: relative;
    width: 100%;
    height: 100%;
    text-align: center;
    transition: transform 0.6s;
    transform-style: preserve-3d;
}

.flashcard.flipped .flashcard-inner {
    transform: rotateY(180deg);
}

.flashcard-front, .flashcard-back {
    position: absolute;
    width: 100%;
    height: 100%;
    backface-visibility: hidden; /* Esconde a parte de trás enquanto não está virada */
    display: flex;
    align-items: center;
    justify-content: center;
    padding: var(--padding-sm);
    box-sizing: border-box; /* Inclui padding no tamanho total */
    font-size: 1.1em;
    font-weight: bold;
}

.flashcard-back {
    transform: rotateY(180deg);
    background-color: var(--secondary-color);
    color: var(--text-color);
    font-weight: normal;
}

.main-footer {
    background-color: var(--primary-color);
    color: white;
    padding: var(--padding-sm);
    text-align: center;
    margin-top: auto; /* Empurra o footer para o final da página */
    box-shadow: 0 -2px 4px var(--shadow-color);
}

/* Responsividade para celular */
@media (max-width: 768px) {
    .main-header h1 {
        font-size: 1.5em;
    }

    .main-header p {
        font-size: 0.9em;
    }

    .flashcard {
        width: 90%; /* Ocupa quase a largura total em telas menores */
        height: 180px; /* Ajusta a altura */
    }

    .flashcard-container {
        padding: var(--padding-sm);
        gap: var(--margin-sm);
    }
}

@media (max-width: 480px) {
    .flashcard {
        width: 95%;
        height: 160px;
    }
    .flashcard-front, .flashcard-back {
        font-size: 1em;
    }
}
```

-----

```javascript
document.addEventListener('DOMContentLoaded', () => {
    // Dados dos flashcards - 9 flashcards originais sobre temas diversos do 2º ano
    const flashcardsData = [
        {
            question: "Qual foi o principal motivo da Revolução Francesa?",
            answer: "Crise econômica, desigualdade social e absolutismo monárquico."
        },
        {
            question: "O que foi o Iluminismo?",
            answer: "Movimento intelectual que valorizava a razão e a ciência em detrimento do pensamento religioso e dogmático."
        },
        {
            question: "Qual a função do DNA?",
            answer: "Armazenar e transmitir informações genéticas."
        },
        {
            question: "O que são vetores em matemática?",
            answer: "Grandezas que possuem módulo (intensidade), direção e sentido."
        },
        {
            question: "Cite um exemplo de reação de oxirredução.",
            answer: "A ferrugem do ferro (oxidação do ferro)."
        },
        {
            question: "O que é globalização?",
            answer: "Processo de integração econômica, social, cultural e política entre diferentes países e regiões do mundo."
        },
        {
            question: "Qual o nome do autor de 'Dom Casmurro'?",
            answer: "Machado de Assis."
        },
        {
            question: "Em que ano o Brasil se tornou independente?",
            answer: "1822."
        },
        {
            question: "O que é o ciclo da água?",
            answer: "Processo contínuo de circulação da água na Terra, envolvendo evaporação, condensação, precipitação e escoamento."
        }
    ];

    const flashcardContainer = document.querySelector('.flashcard-container');

    // Função para criar um flashcard
    function createFlashcard(question, answer) {
        const flashcard = document.createElement('div');
        flashcard.classList.add('flashcard');

        const flashcardInner = document.createElement('div');
        flashcardInner.classList.add('flashcard-inner');

        const flashcardFront = document.createElement('div');
        flashcardFront.classList.add('flashcard-front');
        flashcardFront.textContent = question;

        const flashcardBack = document.createElement('div');
        flashcardBack.classList.add('flashcard-back');
        flashcardBack.textContent = answer;

        flashcardInner.appendChild(flashcardFront);
        flashcardInner.appendChild(flashcardBack);
        flashcard.appendChild(flashcardInner);

        // Adiciona o evento de clique para virar o flashcard
        flashcard.addEventListener('click', () => {
            flashcard.classList.toggle('flipped');
        });

        return flashcard;
    }

    // Adiciona todos os flashcards ao container
    flashcardsData.forEach(data => {
        const flashcardElement = createFlashcard(data.question, data.answer);
        flashcardContainer.appendChild(flashcardElement);
    });
});
```
