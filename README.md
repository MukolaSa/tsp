
<!DOCTYPE html>
<html lang="uk">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Райдомаєзер</title>
  <style>
    body {
      font-family: 'Arial', sans-serif;
      display: flex;
      justify-content: center;
      align-items: center;
      height: 100vh;
      margin: 0;
      background-color: #f4f4f9;
      animation: fadeInBody 1s ease-out;
    }
    
    @keyframes fadeInBody {
      0% {
        opacity: 0;
      }
      100% {
        opacity: 1;
      }
    }

    .card {
      width: 320px;
      padding: 20px;
      border-radius: 10px;
      background-color: white;
      box-shadow: 0 8px 16px rgba(0, 0, 0, 0.1);
      text-align: center;
      animation: slideUp 0.5s ease-out;
    }

    @keyframes slideUp {
      0% {
        transform: translateY(50px);
        opacity: 0;
      }
      100% {
        transform: translateY(0);
        opacity: 1;
      }
    }

    .question {
      font-size: 26px;
      font-weight: 600;
      margin-bottom: 20px;
      animation: fadeIn 1s ease-in-out;
    }

    @keyframes fadeIn {
      0% {
        opacity: 0;
      }
      100% {
        opacity: 1;
      }
    }

    .options {
      display: flex;
      flex-direction: column;
      gap: 15px;
    }

    .option {
      padding: 12px;
      font-size: 18px;
      border-radius: 8px;
      border: 2px solid #ddd;
      background-color: #fafafa;
      cursor: pointer;
      transition: background-color 0.3s, transform 0.3s ease, box-shadow 0.3s ease;
    }

    .option:hover {
      background-color: #f0f0f0;
      transform: scale(1.05);
    }

    .correct {
      background-color: #d4edda;
      border-color: #28a745;
      box-shadow: 0 0 15px rgba(40, 167, 69, 0.5);
      animation: correctAnswer 0.6s ease;
    }

    @keyframes correctAnswer {
      0% {
        transform: scale(0.95);
      }
      100% {
        transform: scale(1);
      }
    }

    .incorrect {
      background-color: #f8d7da;
      border-color: #dc3545;
      box-shadow: 0 0 15px rgba(220, 53, 69, 0.5);
      animation: incorrectAnswer 0.6s ease;
    }

    @keyframes incorrectAnswer {
      0% {
        transform: scale(0.95);
      }
      100% {
        transform: scale(1);
      }
    }

    .disabled {
      pointer-events: none;
    }

    .highlight {
      border-color: #007bff;
      background-color: #cce5ff;
      box-shadow: 0 0 15px rgba(0, 123, 255, 0.3);
      animation: highlightCorrect 0.5s ease;
    }

    @keyframes highlightCorrect {
      0% {
        background-color: #cce5ff;
        border-color: #007bff;
      }
      100% {
        background-color: #d1ecf1;
        border-color: #5bc0de;
      }
    }

    .checkmark {
      display: none;
      font-size: 22px;
      color: #28a745;
      margin-left: 10px;
      animation: fadeInCheckmark 0.3s ease;
    }

    @keyframes fadeInCheckmark {
      0% {
        opacity: 0;
      }
      100% {
        opacity: 1;
      }
    }
  </style>
</head>
<body>
  <div class="card" id="quizCard">
    <div class="question" id="word"></div>
    <div class="options" id="options"></div>
  </div>

  <script>

function shuffleArray(array) {
  for (let i = array.length - 1; i > 0; i--) {
    const j = Math.floor(Math.random() * (i + 1));
    [array[i], array[j]] = [array[j], array[i]];
  }
}

const words = [
{
    "word": "analysis",
    "options": ["методологія", "оцінка", "аналіз", "гіпотеза"],
    "correct": 2
  },
  {
    "word": "assessment",
    "options": ["методологія", "оцінка", "спостереження", "гіпотеза"],
    "correct": 1
  },
  {
    "word": "assumption",
    "options": ["результат", "доказ", "концепція", "припущення"],
    "correct": 3
  },
  {
    "word": "authority",
    "options": ["визнання", "контекст", "деталі", "влада"],
    "correct": 3
  },
  {
    "word": "concept",
    "options": ["гіпотеза", "поняття", "аналіз", "висновок"],
    "correct": 1
  },
  {
    "word": "conclusion",
    "options": ["концепція", "висновок", "грунт", "аналіз"],
    "correct": 1
  },
  {
    "word": "context",
    "options": ["матеріал", "умови", "ситуація", "контекст"],
    "correct": 3
  },
  {
    "word": "criteria",
    "options": ["аналітика", "критерії", "рішення", "параметри"],
    "correct": 1
  },
  {
    "word": "definition",
    "options": ["аналогія", "визначення", "пояснення", "порівняння"],
    "correct": 1
  },
  {
    "word": "evaluation",
    "options": ["аналітика", "методика", "оцінка", "аналіз"],
    "correct": 2
  },
  {
    "word": "hypothesis",
    "options": ["концепція", "гіпотеза", "висновок", "передбачення"],
    "correct": 1
  },
  {
    "word": "interpretation",
    "options": ["аналіз", "інтерпретація", "пояснення", "тлумачення"],
    "correct": 3
  },
  {
    "word": "investigation",
    "options": ["методика", "експеримент", "дослідження", "аналіз"],
    "correct": 2
  },
  {
    "word": "justification",
    "options": ["підтвердження", "висновок", "аналітика", "обґрунтування"],
    "correct": 3
  },
  {
    "word": "methodology",
    "options": ["оцінка", "теорія", "підхід", "методологія"],
    "correct": 3
  },
  {
    "word": "observation",
    "options": ["спостереження", "аналіз", "оцінка", "підхід"],
    "correct": 0
  },
  {
    "word": "prediction",
    "options": ["метод", "аналіз", "гіпотеза", "передбачення"],
    "correct": 3
  },
  {
    "word": "significance",
    "options": ["оцінка", "теорія", "значущість", "важливість"],
    "correct": 2
  },
  {
    "word": "theory",
    "options": ["гіпотеза", "дослідження", "метод", "теорія"],
    "correct": 3
  },
  {
    "word": "variables",
    "options": ["результати", "параметри", "теореми", "змінні"],
    "correct": 3
  },
  {
    "word": "argument",
    "options": ["аргумент", "відповідь", "результат", "доказ"],
    "correct": 0
  },
  {
    "word": "bias",
    "options": ["аналітика", "упередженість", "грунт", "схильність"],
    "correct": 1
  },
  {
    "word": "claim",
    "options": ["доведення", "гіпотеза", "твердження", "аналіз"],
    "correct": 2
  },
  {
    "word": "contradiction",
    "options": ["роз'яснення", "аналогія", "підтвердження", "суперечність"],
    "correct": 3
  },
  {
    "word": "correlation",
    "options": ["змінні", "кореляція", "звіт", "зв'язок"],
    "correct": 1
  },
  {
    "word": "deduction",
    "options": ["аналітика", "грунт", "висновок", "прогноз"],
    "correct": 2
  },
  {
    "word": "evidence",
    "options": ["підтвердження", "аналіз", "докази", "передумови"],
    "correct": 2
  },
  {
    "word": "implication",
    "options": ["наслідки", "результати", "пояснення", "аналіз"],
    "correct": 0
  },
  {
    "word": "inference",
    "options": ["аналітика", "грунт", "гіпотеза", "висновок"],
    "correct": 3
  },
  {
    "word": "justification",
    "options": ["відповідь", "аналіз", "підтвердження", "обґрунтування"],
    "correct": 3
  },
  {
    "word": "logical",
    "options": ["логічний", "пояснення", "грунт", "зміст"],
    "correct": 0
  },
  {
    "word": "paradox",
    "options": ["гіпотеза", "результат", "суперечність", "парадокс"],
    "correct": 3
  },
  {
    "word": "premise",
    "options": ["грунт", "концепція", "передумова", "аналітика"],
    "correct": 2
  },
  {
    "word": "probability",
    "options": ["ймовірність", "результат", "аналіз", "гарантія"],
    "correct": 0
  },
  {
    "word": "reasoning",
    "options": ["висновок", "аналіз", "міркування", "пояснення"],
    "correct": 2
  },
  {
    "word": "reliability",
    "options": ["гіпотеза", "результат", "гарантія", "надійність"],
    "correct": 3
  },
  {
    "word": "skeptical",
    "options": ["позитивний", "скептичний", "негативний", "аналіз"],
    "correct": 1
  },
  {
    "word": "speculation",
    "options": ["грунт", "пояснення", "спекуляція", "передумова"],
    "correct": 2
  },
  {
    "word": "validity",
    "options": ["пояснення", "аналіз", "дійсність", "результат"],
    "correct": 2
  },
  {
    "word": "verification",
    "options": ["перевірка", "доказ", "гіпотеза", "результат"],
    "correct": 0
  },
  {
    "word": "accordingly",
    "options": ["спільно", "наступно", "відповідно", "внаслідок"],
    "correct": 2
  },
  {
    "word": "as a result",
    "options": ["у результаті", "відповідно", "отже", "внаслідок"],
    "correct": 0
  },
  {
    "word": "consequently",
    "options": ["наслідком", "як результат", "внаслідок", "відповідно"],
    "correct": 1
  },
  {
    "word": "due to",
    "options": ["виключно", "насправді", "внаслідок", "через"],
    "correct": 3
  },
  {
    "word": "hence",
    "options": ["відповідно", "отже", "внаслідок", "зрештою"],
    "correct": 1
  },
  {
    "word": "in consequence",
    "options": ["виключно", "наслідок", "в результаті", "через"],
    "correct": 2
  },
  {
    "word": "owing to",
    "options": ["внаслідок", "завдяки", "відповідно", "через"],
    "correct": 1
  },
  {
    "word": "since",
    "options": ["внаслідок", "відповідно", "оскільки", "через"],
    "correct": 2
  },
  {
    "word": "therefore",
    "options": ["наслідок", "відповідно", "отже", "аналітика"],
    "correct": 2
  },
  {
    "word": "thus",
    "options": ["в результаті", "відповідно", "тому", "так"],
    "correct": 3
  },
  {
    "word": "abundant",
    "options": ["щедрий", "рідкісний", "дефіцит", "великі кількості"],
    "correct": 3
  },
  {
    "word": "scarce",
    "options": ["рідкісний", "великі кількості", "щедрий", "дефіцит"],
    "correct": 0
  },
    {
      "word": "accurate",
      "options": ["точний", "невірний", "неспроможний", "недостовірний"],
      "correct": 0
    },
    {
      "word": "inaccurate",
      "options": ["точний", "недостовірний", "невірний", "неспроможний"],
      "correct": 2
    },
    {
      "word": "beneficial",
      "options": ["корисний", "шкідливий", "негативний", "несприятливий"],
      "correct": 0
    },
    {
      "word": "harmful",
      "options": ["корисний", "негативний", "шкідливий","несприятливий"],
      "correct": 2
    },
    {
      "word": "biased",
      "options": [ "неупереджений", "упереджений", "чесний", "справедливий"],
      "correct": 1
    },
    {
      "word": "unbiased",
      "options": ["справедливий", "чесний", "неупереджений", "упереджений"],
      "correct": 2
    },
    {
      "word": "complex",
      "options": ["складний", "простий", "легкий", "без проблем"],
      "correct": 0
    },
    {
      "word": "simple",
      "options": ["легкий", "складний", "без проблем", "простий"],
      "correct": 3
    },
    {
      "word": "consistent",
      "options": ["послідовний", "непослідовний", "незмінний", "рідкий"],
      "correct": 0
    },

    // -----------------------------
    {
      "word": "inconsistent",
      "options": ["непослідовний", "послідовний", "незмінний", "рідкий"],
      "correct": 0
    },
    {
      "word": "explicit",
      "options": ["ясний", "неявний", "неоднозначний", "туманний"],
      "correct": 0
    },
    {
      "word": "implicit",
      "options": ["неявний", "ясний", "неоднозначний", "туманний"],
      "correct": 0
    },
    {
      "word": "hypothetical",
      "options": ["гіпотетичний", "фактичний", "реальний", "точний"],
      "correct": 0
    },
    {
      "word": "factual",
      "options": ["фактичний", "гіпотетичний", "реальний", "точний"],
      "correct": 0
    },
    {
      "word": "logical",
      "options": ["логічний", "нелогічний", "безсистемний", "незв'язний"],
      "correct": 0
    },
    {
      "word": "illogical",
      "options": ["нелогічний", "логічний", "безсистемний", "незв'язний"],
      "correct": 0
    },
    {
      "word": "relevant",
      "options": ["доречний", "недоречний", "відповідний", "невідповідний"],
      "correct": 0
    },
    {
      "word": "irrelevant",
      "options": ["недоречний", "доречний", "відповідний", "невідповідний"],
      "correct": 0
    },
    {
      "word": "abstract",
      "options": ["абстрактний", "конкретний", "реальний", "відчутний"],
      "correct": 0
    },
    {
      "word": "argumentation",
      "options": ["аргументація", "роздум", "аналіз", "обговорення"],
      "correct": 0
    },
    {
      "word": "coherence",
      "options": ["узгодженість", "розбіжність", "суперечність", "несумісність"],
      "correct": 0
    },
    {
      "word": "comparison",
      "options": ["порівняння", "контраст", "паралель", "зіставлення"],
      "correct": 0
    },
    {
      "word": "contradiction",
      "options": ["суперечність", "згода", "визнання", "розуміння"],
      "correct": 0
    },
    {
      "word": "contrast",
      "options": ["контраст", "схожість", "паралель", "зіставлення"],
      "correct": 0
    },
    {
      "word": "emphasis",
      "options": ["акцент", "відсутність", "нейтральність", "простота"],
      "correct": 0
    },
    {
      "word": "implication",
      "options": ["наслідки", "пояснення", "висновок", "причина"],
      "correct": 0
    },
    {
      "word": "inference",
      "options": ["висновок", "пояснення", "грунт", "аналітика"],
      "correct": 0
    },
    {
      "word": "rhetorical",
      "options": ["риторичний", "прагматичний", "аналіз", "безособовий"],
      "correct": 0
    },
    {
      "word": "bite off more than you can chew",
      "options": ["переборщити", "роздумувати", "відмовитися", "вибачати"],
      "correct": 0
    },
    {
      "word": "beat around the bush",
      "options": ["ходити навколо", "мовчати", "відвертатися", "уникати прямої відповіді"],
      "correct": 3
    },
    {
      "word": "cast pearls before swine",
      "options": ["давати дари тим, хто їх не цінує", "втрачаючи час", "брехати", "не помічати"],
      "correct": 0
    },
    {
      "word": "cross that bridge when you come to it",
      "options": ["вирішити проблему по мірі її виникнення", "спокійно чекати", "відкласти", "не хвилюватися"],
      "correct": 0
    },
    {
      "word": "judge a book by its cover",
      "options": ["оцінювати книгу за її обкладинкою", "визначати зміст", "порівнювати", "робити висновки по зовнішності"],
      "correct": 0
    },
    {
      "word": "the tip of the iceberg",
      "options": ["тільки верхівка айсберга", "невелика частина великого", "початок проблеми", "основна частина"],
      "correct": 0
    },
    {
      "word": "atom",
      "options": ["молекула", "атом", "гена", "хімічна сполука"],
      "correct": 1
    },
    {
      "word": "biological",
      "options": ["біологічний", "хімічний", "фізичний", "географічний"],
      "correct": 0
    },
    {
      "word": "catalyst",
      "options": ["реагент", "каталізатор", "речовина", "молекула"],
      "correct": 0
    },
    {
      "word": "chromosome",
      "options": ["мітохондрія", "хромосома", "ДНК", "ген"],
      "correct": 1
    },
    {
      "word": "compound",
      "options": ["сполука", "елемент", "молекула", "атом"],
      "correct": 0
    },
    {
      "word": "enzyme",
      "options": ["фермент", "активатор", "реакція", "сполука"],
      "correct": 0
    },
    {
      "word": "genetic",
      "options": ["генетичний", "біологічний", "фізичний", "математичний"],
      "correct": 0
    },
    {
      "word": "molecule",
      "options": ["молекула", "атом", "хімічна сполука", "молекулярна структура"],
      "correct": 0
    },
    {
      "word": "mutation",
      "options": ["мутація", "відмінність", "фенотип", "генотип"],
      "correct": 0
    },
    {
      "word": "reaction",
      "options": ["реакція", "відповідь", "реактивність", "результат"],
      "correct": 0
    },
    {
      "word": "accordingly",
      "options": ["відповідно", "однак", "насправді", "в цілому"],
      "correct": 0
    },
    {
      "word": "arguably",
      "options": ["сумнівно", "можливо", "відомо", "спірно"],
      "correct": 1
    },
    {
      "word": "consequently",
      "options": ["відповідно", "наслідком", "також", "зокрема"],
      "correct": 1
    },
    {
      "word": "evidently",
      "options": ["очевидно", "по суті", "потрібно", "можливо"],
      "correct": 0
    },
    {
      "word": "furthermore",
      "options": ["додатково", "тим більше", "при тому", "разом з тим"],
      "correct": 0
    },
    {
      "word": "namely",
      "options": ["тобто", "наприклад", "оскільки", "після того"],
      "correct": 0
    },
    {
      "word": "notably",
      "options": ["особливо", "відомо", "наприклад", "насамперед"],
      "correct": 0
    },
    {
      "word": "overall",
      "options": ["в загальному", "загалом", "зокрема", "порівняно"],
      "correct": 0
    },
    {
      "word": "presumably",
      "options": ["ймовірно", "вірогідно", "схоже", "можливо"],
      "correct": 0
    },
    {
      "word": "ultimately",
      "options": ["зрештою", "в кінцевому підсумку", "насамперед", "спочатку"],
      "correct": 0
    },
    {
      "word": "analyze",
      "options": ["аналізувати", "порівнювати", "визначати", "відповідати"],
      "correct": 0
    },
    {
      "word": "argue",
      "options": ["сперечатися", "аргументувати", "оцінювати", "пояснювати"],
      "correct": 0
    },
    {
      "word": "assume",
      "options": ["припускати", "оцінювати", "обговорювати", "аналізувати"],
      "correct": 0
    },
    {
      "word": "compare",
      "options": ["порівнювати", "відміняти", "анулювати", "описувати"],
      "correct": 0
    },
    {
      "word": "conclude",
      "options": ["робити висновок", "починати", "закінчувати", "оцінювати"],
      "correct": 0
    },
    {
      "word": "biodiversity",
      "options": ["біорізноманіття", "консервація", "вимирання", "забруднення"],
      "correct": 0
    },
    {
      "word": "conservation",
      "options": ["консервація", "вираження", "екосистема", "викиди"],
      "correct": 0
    },
    {
      "word": "deforestation",
      "options": ["лісозаготівля", "вирубка лісів", "пожежа", "вимирання"],
      "correct": 0
    },
    {
      "word": "ecosystem",
      "options": ["екосистема", "біорізноманіття", "викиди", "вимирання"],
      "correct": 0
    },
    {
      "word": "emissions",
      "options": ["викиди", "відновлення", "повторення", "експансія"],
      "correct": 0
    },
    {
      "word": "extinction",
      "options": ["вимирання", "консервація", "екосистема", "біорізноманіття"],
      "correct": 0
    },
    {
      "word": "fossil fuels",
      "options": ["викопне паливо", "відновлювальні джерела енергії", "забруднення", "викиди"],
      "correct": 0
    },
    {
      "word": "greenhouse effect",
      "options": ["парниковий ефект", "озоновий шар", "кільце", "забруднення"],
      "correct": 0
    },
    {
      "word": "pollution",
      "options": ["забруднення", "біорізноманіття", "відновлення", "екосистема"],
      "correct": 0
    },
    {
      "word": "sustainability",
      "options": ["стійкість", "відновлюваність", "дефіцит", "необхідність"],
      "correct": 0
    },
    {
      "word": "ambitious",
      "options": ["амбіційний", "терплячий", "практичний", "відкритий"],
      "correct": 0
    },
    {
      "word": "conscientious",
      "options": ["совісний", "недбалий", "сміливий", "щирий"],
      "correct": 0
    },
    {
      "word": "courteous",
      "options": ["ввічливий", "неприязний", "замкнутий", "сміливий"],
      "correct": 0
    },
    {
      "word": "decisive",
      "options": ["рішучий", "нерішучий", "підступний", "запізнілий"],
      "correct": 0
    },
    {
      "word": "diligent",
      "options": ["працьовитий", "ледачий", "недбалий", "відповідальний"],
      "correct": 0
    },
    {
      "word": "empathetic",
      "options": ["емпатичний", "безсумнівний", "недбалий", "грубуватий"],
      "correct": 0
    },
    {
      "word": "gullible",
      "options": ["докучливий", "легковірний", "підозрілий", "замкнутий"],
      "correct": 0
    },
    {
      "word": "innovative",
      "options": ["інноваційний", "старомодний", "нудний", "нецікавий"],
      "correct": 0
    },
    {
      "word": "meticulous",
      "options": ["тщеславний", "ретельний", "грубуватий", "недбалий"],
      "correct": 1
    },
    {
      "word": "resilient",
      "options": ["стійкий", "слабкий", "незмінний", "безпорадний"],
      "correct": 0
    },
    {
      "word": "ambivalent",
      "options": ["амбіваентний", "впевнений", "негативний", "дефіцит"],
      "correct": 0
    },
    {
      "word": "conundrum",
      "options": ["загадка", "відповідь", "спрощення", "виклик"],
      "correct": 0
    },
    {
      "word": "dichotomy",
      "options": ["поділ", "злиття", "спільність", "взаємопідтримка"],
      "correct": 0
    },
    {
      "word": "exacerbate",
      "options": ["посилювати", "зменшувати", "недооцінювати", "підтримувати"],
      "correct": 0
    },
    {
      "word": "inadvertent",
      "options": ["неумисний", "очевидний", "цілеспрямований", "випадковий"],
      "correct": 0
    },
    {
      "word": "juxtaposition",
      "options": ["порівняння", "виправдання", "навпаки", "порушення"],
      "correct": 0
    },
    {
      "word": "misinterpretation",
      "options": ["неправильне тлумачення", "точне тлумачення", "перекручування", "відсутність тлумачення"],
      "correct": 0
    },
    {
      "word": "quintessential",
      "options": ["основний", "допоміжний", "застарілий", "невизначений"],
      "correct": 0
    },
    {
      "word": "superfluous",
      "options": ["надмірний", "необхідний", "економний", "відсутній"],
      "correct": 0
    },
    {
      "word": "ubiquitous",
      "options": ["всезвісний", "рідкісний", "складний", "незв'язний"],
      "correct": 0
    },
    {
      "word": "apprehensive",
      "options": ["тривожний", "спокійний", "рішучий", "дбайливий"],
      "correct": 0
    },
    {
      "word": "ecstatic",
      "options": ["в екстазі", "підозрілий", "недовірливий", "сумний"],
      "correct": 0
    },
    {
      "word": "indifferent",
      "options": ["байдужий", "закоханий", "емоційний", "заперечливий"],
      "correct": 0
    },
    {
      "word": "nostalgic",
      "options": ["ностальгічний", "енергійний", "емоційний", "нехотячи"],
      "correct": 0
    },
    {
      "word": "perplexed",
      "options": ["здивований", "смішний", "заплутаний", "спокійний"],
      "correct": 2
    },
    {
      "word": "beforehand",
      "options": ["заздалегідь", "після", "паралельно", "несподівано"],
      "correct": 0
    },
    {
      "word": "concurrently",
      "options": ["паралельно", "запізно", "наступно", "одночасно"],
      "correct": 0
    },
    {
      "word": "forthcoming",
      "options": ["майбутній", "поточний", "запізнілий", "заблокований"],
      "correct": 0
    },
    {
      "word": "imminent",
      "options": ["невідкладний", "віддалений", "неясний", "обов'язковий"],
      "correct": 0
    },
    {
      "word": "intermittent",
      "options": ["переривчастий", "постійний", "стабільний", "повторюваний"],
      "correct": 0
    },
    {
      "word": "perpetual",
      "options": ["безперервний", "часовий", "перемінний", "тимчасовий"],
      "correct": 0
    },
    {
      "word": "prior",
      "options": ["попередній", "запізнілий", "важливий", "неочікуваний"],
      "correct": 0
    },
    {
      "word": "sequential",
      "options": ["послідовний", "довільний", "випадковий", "неповний"],
      "correct": 0
    },
    {
      "word": "subsequent",
      "options": ["наступний", "попередній", "недавній", "незмінний"],
      "correct": 0
    },
    {
      "word": "unprecedented",
      "options": ["безпрецедентний", "незвичний", "стандартний", "очікуваний"],
      "correct": 0
    }
    
  ]









    function getRandomWord() {
      const randomIndex = Math.floor(Math.random() * words.length);
      return words[randomIndex];
    }

    function displayWord() {
      const currentWord = getRandomWord();
      const wordElement = document.getElementById('word');
      const optionsElement = document.getElementById('options');
      optionsElement.innerHTML = '';

      wordElement.textContent = `Що означає слово "${currentWord.word}"?`;

      currentWord.options.forEach((option, index) => {
        const optionElement = document.createElement('div');
        optionElement.textContent = option;
        optionElement.classList.add('option');
        optionElement.onclick = () => checkAnswer(index, currentWord.correct, optionElement, currentWord);
        optionsElement.appendChild(optionElement);
      });
    }

    function checkAnswer(selectedIndex, correctIndex, optionElement, currentWord) {
      const allOptions = document.querySelectorAll('.option');
      allOptions.forEach(option => option.classList.add('disabled'));

      if (selectedIndex === correctIndex) {
        optionElement.classList.add('correct');
        const checkmark = document.createElement('span');
        checkmark.classList.add('checkmark');
        checkmark.textContent = '✔';
        optionElement.appendChild(checkmark);
        checkmark.style.display = 'inline';
      } else {
        optionElement.classList.add('incorrect');
        const correctOption = allOptions[correctIndex];
        correctOption.classList.add('highlight');
      }

      setTimeout(() => displayWord(), 2000);
    }

    displayWord();
  </script>
</body>
</html>