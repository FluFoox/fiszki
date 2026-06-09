<!DOCTYPE html>
<html lang="pl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Mamma mia PNOM!?!?</title>
    <style>
        :root {
            --bg-color: #121212;
            --card-bg: #1e1e1e;
            --text-color: #e0e0e0;
            --text-muted: #a0a0a0;
            --primary: #bb86fc;
            --primary-variant: #3700b3;
            --success: #4caf50;
            --error: #f44336;
            --border: #333333;
            --input-bg: #252525;
        }

        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background-color: var(--bg-color);
            color: var(--text-color);
            margin: 0;
            padding: 20px;
            display: flex;
            flex-direction: column;
            align-items: center;
            min-height: 100vh;
        }

        .container {
            max-width: 750px;
            width: 100%;
            background: var(--card-bg);
            border-radius: 12px;
            padding: 30px;
            box-shadow: 0 8px 24px rgba(0,0,0,0.5);
            border: 1px solid var(--border);
            margin-top: 20px;
            box-sizing: border-box;
        }

        h1 {
            text-align: center;
            color: var(--primary);
            margin-bottom: 5px;
            font-size: 1.8rem;
        }

        .subtitle {
            text-align: center;
            color: var(--text-muted);
            font-size: 0.9rem;
            margin-bottom: 25px;
        }

        /* Sekcja kontrolna / Menu */
        .controls {
            display: flex;
            flex-direction: column;
            gap: 15px;
            margin-bottom: 25px;
            padding-bottom: 20px;
            border-bottom: 1px solid var(--border);
        }

        .controls-row {
            display: flex;
            justify-content: space-between;
            align-items: center;
            flex-wrap: wrap;
            gap: 12px;
        }

        .modes {
            display: flex;
            gap: 10px;
        }

        button, select, input {
            background-color: var(--input-bg);
            color: var(--text-color);
            border: 1px solid var(--border);
            padding: 10px 16px;
            border-radius: 6px;
            cursor: pointer;
            font-weight: 600;
            font-family: inherit;
            transition: all 0.2s ease;
            box-sizing: border-box;
        }

        input[type="text"] {
            cursor: text;
            font-weight: normal;
            flex-grow: 1;
            min-width: 200px;
        }

        input:focus, select:focus {
            outline: none;
            border-color: var(--primary);
        }

        button:hover:not(:disabled) {
            background-color: #3d3d3d;
            border-color: var(--primary);
        }

        button.active {
            background-color: var(--primary);
            color: #000;
            border-color: var(--primary);
        }

        .stats {
            font-size: 0.95rem;
            color: var(--text-muted);
            background: #252525;
            padding: 8px 14px;
            border-radius: 20px;
            border: 1px solid var(--border);
        }

        .search-jump-container {
            display: flex;
            gap: 12px;
            width: 100%;
            flex-wrap: wrap;
        }

        /* Karta pytania */
        .question-box {
            margin-bottom: 25px;
        }

        .q-number {
            color: var(--primary);
            font-size: 0.85rem;
            text-transform: uppercase;
            letter-spacing: 1px;
            margin-bottom: 8px;
            font-weight: bold;
        }

        .q-text {
            font-size: 1.2rem;
            line-height: 1.5;
            margin-bottom: 20px;
            font-weight: 500;
        }

        .ai-badge {
            display: inline-block;
            background: #332211;
            color: #ffb74d;
            font-size: 0.75rem;
            padding: 2px 6px;
            border-radius: 4px;
            margin-left: 8px;
            border: 1px solid #e65100;
            vertical-align: middle;
        }

        /* Opcje odpowiedzi */
        .options-list {
            display: flex;
            flex-direction: column;
            gap: 12px;
        }

        .option {
            background-color: #252525;
            border: 1px solid var(--border);
            border-radius: 8px;
            padding: 15px 20px;
            cursor: pointer;
            transition: all 0.2s ease;
            display: flex;
            align-items: center;
            font-size: 1.05rem;
            font-weight: normal; /* Normalna waga czcionki na start */
        }

        .option:hover:not(.disabled) {
            background-color: #2d2d2d;
            border-color: #555;
            transform: translateX(2px);
        }

        .option.correct {
            background-color: rgba(76, 175, 80, 0.15) !important;
            border-color: var(--success) !important;
            color: #81c784;
            font-weight: bold; /* Pogrubia się dopiero po kliknięciu jako wyróżnienie dobrej odpowiedzi */
        }

        .option.wrong {
            background-color: rgba(244, 67, 54, 0.15) !important;
            border-color: var(--error) !important;
            color: #e57373;
        }

        .option.disabled {
            cursor: not-allowed;
        }

        /* Przycisk Dalej */
        .actions {
            display: flex;
            justify-content: flex-end;
            margin-top: 25px;
        }

        #next-btn {
            background-color: var(--primary-variant);
            color: #fff;
            border: none;
            padding: 12px 24px;
            font-size: 1rem;
            border-radius: 6px;
        }

        #next-btn:hover:not(:disabled) {
            background-color: #4b01e3;
        }

        #next-btn:disabled {
            background-color: #222;
            color: #555;
            cursor: not-allowed;
            border: 1px solid #333;
        }

        .empty-state {
            text-align: center;
            padding: 40px 20px;
            color: var(--text-muted);
        }
    </style>
</head>
<body>

    <div class="container">
        <h1>Fiszki Egzaminacyjne</h1>
        <div class="subtitle">Inżynieria Materiałowa & Obróbka Cieplna</div>

        <div class="controls">
            <div class="controls-row">
                <div class="modes">
                    <button id="mode-normal" class="active" onclick="setMode('normal')">Tryb zwykły</button>
                    <button id="mode-learn" onclick="setMode('learn')">Tryb nauki (<span id="review-count">0</span>)</button>
                </div>
                <div>
                    <button id="shuffle-btn" onclick="toggleShuffle()">Losowe pytania: WYŁ</button>
                </div>
                <div class="stats">
                    Pytanie: <span id="current-index">0</span>/<span id="total-count">0</span>
                </div>
            </div>

            <div class="search-jump-container">
                <input type="text" id="search-input" placeholder="Szukaj pytania po słowach..." oninput="handleSearch()">
                <select id="jump-select" onchange="handleJumpToQuestion()">
                    <option value="">Skocz do pytania...</option>
                </select>
            </div>
        </div>

        <div id="quiz-area">
            <div class="question-box">
                <div class="q-number" id="question-id">Pytanie 1</div>
                <div class="q-text" id="question-text">Treść pytania pojawi się tutaj...</div>
            </div>

            <div class="options-list" id="options-container">
                </div>

            <div class="actions">
                <button id="next-btn" onclick="nextQuestion()" disabled>Następne pytanie &rarr;</button>
            </div>
        </div>
    </div>

    <script>
        // Baza wszystkich 107 pytań na podstawie obrazów
        const database = [
            { id: 1, q: "Odpuszczanie stali polega na:", type: "normal", answers: ["Wyżarzaniu stali odkształconej plastycznie", "Wyżarzaniu stali zahartowanej", "Wyżarzaniu stali w celu uzyskania drobnego ziarna"], correct: 1 },
            { id: 2, q: "Podstawowym mechanizmem umocnienia duraluminium jest:", type: "normal", answers: ["Umocnienie wydzieleniowe", "Umocnienie roztworowe", "Umocnienie odkształceniowe"], correct: 0 },
            { id: 3, q: "Podstawowym pierwiastkiem, który przyczynia się do wzrostu odporności korozyjnej stali jest:", type: "normal", answers: ["Ni", "Mo", "Cr (chrom)"], correct: 2 },
            { id: 4, q: "Perlit w stalach tworzy się:", type: "normal", answers: ["Podczas szybkiego chłodzenia austenitu", "Podczas odpuszczania zahartowanej stali", "Podczas powolnego chłodzenia austenitu *(w wyniku przemiany eutektoidalnej)"], correct: 2 },
            { id: 5, q: "Wzrost zawartości perlitu w stali spowoduje:", type: "normal", answers: ["Zwiększenie udarności", "Zwiększenie wytrzymałości, spadek sprężystości", "Obniżenie temperatury przejścia w stan kruchy"], correct: 1 },
            { id: 6, q: "Najkrótsza definicja martenzytu to:", type: "normal", answers: ["Jest to roztwór stały węgla w żelazie γ (gamma)", "Jest to przesycony roztwór stały węgla w żelazie α (alfa)", "Jest to odmiana alotropowa żelaza"], correct: 1 },
            { id: 7, q: "Stale stosowane na duże konstrukcje wymagające spawania (mosty, budynki, rurociągi itp.) powinny zawierać:", type: "normal", answers: ["Mało węgla", "Możliwie dużo węgla", "Dowolną ilość węgla (ilość węgla nie ma znaczenia)"], correct: 0 },
            { id: 8, q: "Normalizowanie to obróbka cieplna stali prowadząca do:", type: "normal", answers: ["Otrzymania małego równoosiowego ziarna", "Rekrystalizacji materiału odkształconego", "Usunięcia naprężeń hartowniczych"], correct: 2 },
            { id: 9, q: "Temperatura przejścia w stan kruchy stopów aluminium zależy od:", type: "normal", answers: ["Zawartości w stopie pierwiastków stopowych", "Wielkości ziarna", "Stopy Al nie są kruche w niskiej temperaturze"], correct: 0 },
            { id: 10, q: "Którego pierwiastka należy dodać do stali, aby otrzymać austenit w temperaturze pokojowej?", type: "normal", answers: ["miedzi", "niklu", "chromu"], correct: 1 },
            { id: 11, q: "Zjawisko tzw. twardości wtórnej wykorzystuje się:", type: "normal", answers: ["W stopach aluminium utwardzanych wydzieleniowo", "W blachach na karoserie samochodowe", "W stalach narzędziowych"], correct: 2 },
            { id: 12, q: "Który z mechanizmów umocnienia stali jednocześnie zwiększa granicę plastyczności i obniża temperaturę przejścia w stan kruchy?", type: "normal", answers: ["Umocnienie cząstkami dyspersyjnymi", "Umocnienie odkształceniowe", "Umocnienie przez rozdrobnienie ziarna"], correct: 2 },
            { id: 13, q: "Która z faz jest najtwardsza?", type: "normal", answers: ["cementyt", "ferryt", "austenit"], correct: 0 },
            { id: 14, q: "Który z poniższych wzorów opisuje regułę faz Gibbsa przy stałym ciśnieniu?", type: "normal", answers: ["s = n - f + 1", "s = n - f + 2", "s = n - f"], correct: 0 },
            { id: 15, q: "Na czym polega przemiana eutektyczna przy chłodzeniu?", type: "normal", answers: ["w stałej temperaturze z ciała stałego tworzą się dwie nowe fazy stałe", "w stałej temperaturze tworzy się nowa faza stała i ciecz o innym składzie", "w stałej temperaturze z cieczy tworzą się dwie fazy stałe"], correct: 2 },
            { id: 16, q: "Ledeburyt to:", type: "normal", answers: ["mieszanina eutektyczna austenitu i ferrytu", "mieszanina eutektyczna austenitu i cementytu", "mieszanina eutektoidalna austenitu i cementytu"], correct: 1 },
            { id: 17, q: "Stopów aluminium nie można hartować ponieważ:", type: "normal", answers: ["Aluminium nie ma odmian alotropowych", "Stopy Al nie zawierają węgla", "Aluminium ma niską temperaturę topnienia"], correct: 1 },
            { id: 18, q: "Wybierz temperaturę, z której hartuje się stal niestopową o zawartości węgla 1,1%", type: "normal", answers: ["850°C", "757°C", "727°C"], correct: 2 },
            { id: 19, q: "Wytrzymałość na rozciąganie Rm to naprężenie rozciągające:", type: "normal", answers: ["odpowiadające największej sile obciążającej uzyskanej podczas przeprowadzenia próby", "rzeczywiste, występujące w przekroju poprzecznym próbki w miejscu przewężenia w chwili rozerwania", "przy osiągnięciu którego następuje trwałe odkształcenie plastyczne materiału"], correct: 0 },
            { id: 20, q: "Które wiązanie między atomami (cząsteczkami) jest najsłabsze?", type: "normal", answers: ["Van der Waalsa (wtórne)", "Kowalencyjne", "Metaliczne"], correct: 0 },
            { id: 21, q: "Nadstopy (superstopy) są to materiały stosowane:", type: "normal", answers: ["W temperaturze kriogenicznej", "Na wytrzymałe narzędzia", "W wysokiej temperaturze"], correct: 2 },
            { id: 22, q: "Podstawowym mechanizmem umocnienia duraluminium jest:", type: "normal", answers: ["Umocnienie odkształceniowe (dyslokacyjne)", "Umocnienie przez rozdrobnienie ziarna", "Umocnienie wydzieleniowe"], correct: 2 },
            { id: 23, q: "Ulepszanie cieplne stali jest to proces polegający na:", type: "normal", answers: ["Połączeniu procesów hartowania i odpuszczania", "Połączeniu procesów odkształcenia i rekrystalizacji", "Obróbce cieplnej stali bezpośrednio po odlewaniu"], correct: 0 },
            { id: 24, q: "Najbardziej skutecznym mechanizmem umocnienia metali przeznaczonych do zastosowań w podwyższonej temperaturze jest:", type: "normal", answers: ["Umocnienie granicami ziarn (przez rozdrobnienie ziarna)", "Umocnienie dyslokacjami (umocnienie odkształceniowe)", "Umocnienie cząstkami (umocnienie dyspersyjne)"], correct: 2 },
            { id: 25, q: "Które z poniższych stwierdzeń NIE jest prawdziwe?", type: "normal", answers: ["Zmniejszenie ziarna w stali powoduje obniżenie temperatury przejścia w stan kruchy", "Zmniejszenie ziarna w stali powoduje zwiększenie odporności na pełzanie", "Zmniejszenie ziarna w stali powoduje wzrost granicy plastyczności"], correct: 1 },
            { id: 26, q: "Temperaturę przejścia w stan kruchy wykazują metale:", type: "normal", answers: ["O strukturze krystalicznej regularnej przestrzennie centrowanej A2", "O strukturze krystalicznej regularnej ściennie centrowanej A1", "O strukturze krystalicznej heksagonalnej zwartej A3"], correct: 0 },
            { id: 27, q: "W jakich jednostkach mierzy się udarność?", type: "normal", answers: ["Energii", "Naprężenia", "Siły"], correct: 0 },
            { id: 28, q: "Mosiądze są to stopy miedzi z:", type: "normal", answers: ["Cyną", "Cynkiem", "Aluminium"], correct: 1 },
            { id: 29, q: "Współczynnik umocnienia stali stosowanych do tłoczenia, np. karoserii samochodowych, powinien być:", type: "normal", answers: ["Jak najmniejszy", "Możliwie duży", "Jego wartość nie ma znaczenia"], correct: 1 },
            { id: 30, q: "Stale dwufazowe (dual phase – DP) stosowane na elementy karoserii samochodowych charakteryzują się:", type: "normal", answers: ["Niską granicą plastyczności i wysoką wytrzymałością", "Niską granicą plastyczności i niską wytrzymałością", "Wysoką granicą plastyczności i wysoką wytrzymałością"], correct: 0 },
            { id: 31, q: "Podstawowymi pierwiastkami stopowymi stali szybkotnących są:", type: "normal", answers: ["Chrom lub nikiel", "Tytan lub niob", "Wolfram lub molibden"], correct: 2 },
            { id: 32, q: "Duże ziarno w materiałach metalicznych jest korzystne, gdy materiał jest stosowany w:", type: "normal", answers: ["Podwyższonej temperaturze", "Temperaturze kriogenicznej", "Warunkach zmiennych naprężeń"], correct: 2 },
            { id: 33, q: "Główną cechą stali o dobrej spawalności jest:", type: "normal", answers: ["Wysoka zawartość pierwiastków stopowych", "Niska zawartość węgla", "Struktura drobnoziarnista"], correct: 1 },
            { id: 34, q: "Modyfikację siluminów prowadzi się celem:", type: "normal", answers: ["poprawy odporności korozyjnej stopu", "poprawy własności mechanicznych", "zwiększenia oporności właściwej stopu"], correct: 1 },
            { id: 35, q: "Które materiały nazywane są piezoelektrykami?", type: "normal", answers: ["takie, w których pod wpływem naprężeń mechanicznych powstaje pole elektryczne i na odwrót", "takie, w których pod wpływem zmiany temperatury powstają na powierzchniach przeciwne ładunki elektryczne", "takie, w których pod wpływem zewnętrznego pola elektrycznego zmienia się stała dielektryczna"], correct: 0 },
            { id: 36, q: "Gdzie głównie stosuje się materiały magnetyczne twarde?", type: "normal", answers: ["do wytwarzania stałych magnesów", "do wytwarzania nośników pamięci", "do wytwarzania blach transformatorowych"], correct: 0 },
            { id: 37, q: "Co to jest temperatura Curie w odniesieniu do materiałów magnetycznych?", type: "normal", answers: ["temperatura, powyżej której ferromagnetyk staje się paramagnetykiem", "temperatura, powyżej której paramagnetyk staje się diamagnetykiem", "temperatura, poniżej której diamagnetyk staje się ferromagnetykiem"], correct: 0 },
            { id: 38, q: "Które materiały charakteryzują się dużymi współczynnikami rozszerzalności cieplnej?", type: "normal", answers: ["materiały o wysokiej temperaturze topnienia", "materiały o niskiej temperaturze topnienia", "materiały o dużej wartości przewodności cieplnej"], correct: 0 },
            { id: 39, q: "W wielkim piecu produkujemy docelowo:", type: "normal", answers: ["stal niskostopową", "surówkę przeróbczą", "jednocześnie surówkę przeróbczą i stal niskostopową"], correct: 1 },
            { id: 40, q: "Wsadem do konwertora tlenowego jest:", type: "normal", answers: ["tylko surówka żelaza", "surówka żelaza i złom stalowy", "tylko złom stalowy"], correct: 1 },
            { id: 41, q: "Wyrobami walcowanymi na zimno są głównie:", type: "normal", answers: ["blachy cienkie i taśmy", "szyny kolejowe", "pręty do zbrojenia betonu"], correct: 0 },
            { id: 42, q: "Jaka jest prawidłowa kolejność przebiegu procesów metalurgicznych:", type: "normal", answers: ["Spiekalnia, obróbka pozapiecowa stali, wielki piec, konwertor, COS, walcowanie", "Spiekalnia, wielki piec, konwertor, obróbka pozapiecowa stali, COS, walcowanie", "Wielki piec, konwertor, spiekalnia, obróbka pozapiecowa stali, COS, walcowanie"], correct: 1 },
            { id: 43, q: "Które z wymienionych parametrów służyć mogą do oceny plastyczności materiałów?", type: "normal", answers: ["granica plastyczności materiału", "wytrzymałość materiału na rozciąganie", "wydłużenie otrzymane z próby jednoosiowego rozciągania"], correct: 2 },
            { id: 44, q: "Zabieg patentowania stosuje się w technologii wytwarzania:", type: "normal", answers: ["prętów zbrojeniowych", "rur", "drutów"], correct: 2 },
            { id: 45, q: "Słabą zależność od mikrostruktury stali wykazuje:", type: "normal", answers: ["granica plastyczności", "moduł Younga", "twardość"], correct: 1 },
            { id: 46, q: "Ogólnie dużą odpornością na pękanie (KIc) charakteryzują się:", type: "normal", answers: ["stopy metali", "ceramiki", "polimery"], correct: 1 },
            { id: 47, q: "Równowagowe stężenie defektów punktowych:", type: "normal", answers: ["zmniejsza się ze wzrostem temperatury", "jest niezależne od temperatury", "zwiększa się ze wzrostem temperatury"], correct: 2 },
            { id: 48, q: "Odkształcaniu przez bliźniakowanie sprzyja:", type: "normal", answers: ["wysoka temperatura odkształcenia", "niska temperatura odkształcenia", "małe ziarno odkształcanego materiału"], correct: 1 },
            { id: 49, q: "Ogólnie twardość metali:", type: "normal", answers: ["zwiększa się ze wzrostem wytrzymałości", "maleje ze wzrostem wytrzymałości", "jest niezależna od wytrzymałości"], correct: 0 },
            { id: 50, q: "Siłą pędną rekrystalizacji pierwotnej odkształconych plastycznie metali jest:", type: "normal", answers: ["energia defektów punktowych utworzonych podczas odkształcania", "energia dyslokacji zmagazynowanych podczas odkształcenia", "energia granic ziarn"], correct: 1 },
            { id: 51, q: "Anizotropia normalna w blachach głębokotłocznych:", type: "normal", answers: ["pozwala na nadawanie złożonych kształtów wyrobom z tych blach", "nie ma związku z teksturą tłoczonej blachy", "jest zjawiskiem negatywnym i wpływa na wielkość wad powstałych w trakcie tłoczenia"], correct: 0 },
            { id: 52, q: "Osnowa w stopie umacnianym dyspersyjnie:", type: "normal", answers: ["powinna być znacznie twardsza niż występujące w niej cząstki", "jest odpowiedzialna za jego umocnienie", "jest fazą ciągłą stanowiącą znaczną jego objętość"], correct: 1 },
            { id: 53, q: "W trakcie jednoosiowego rozciągania mosiężnej próbki wytrzymałościowej:", type: "normal", answers: ["obserwuje się zarówno wzrost wytrzymałości jak i plastyczności stopu", "stop odkształca się jedynie sprężyście", "po przekroczeniu Rm naprężenie rzeczywiste gwałtownie maleje"], correct: 0 },
            { id: 54, q: "Na podstawie współczynnika rozszerzalności liniowej można wnioskować o:", type: "normal", answers: ["przewodnictwie cieplnym", "sile wiązań międzyatomowych", "rezystywności"], correct: 1 },
            { id: 55, q: "Po azotowaniu stali:", type: "normal", answers: ["stosuje się ulepszanie cieplne", "stosuje się przesycanie i starzenie", "nie stosuje się obróbki cieplnej"], correct: 2 },
            { id: 56, q: "Po hartowaniu stali:", type: "normal", answers: ["objętość próbki zwiększa się", "objętość próbki zmniejsza się", "objętość próbki nie zmienia się"], correct: 0 },
            { id: 57, q: "W celu zapewnienia stali wysokiej twardości i odporności na ścieranie:", type: "normal", answers: ["stal nawęgla się po uprzednim jej zahartowaniu", "stal po nawęglaniu poddaje się ulepszaniu cieplnemu", "stal po nawęglaniu hartuje się i nisko odpuszcza"], correct: 2 },
            { id: 58, q: "Czy materiały funkcjonalne zmieniają swoje własności pod wpływem przyłożonego do takich materiałów zewnętrznego pola mechanicznego, elektrycznego lub magnetycznego?", type: "normal", answers: ["Tak. Takie zachowanie materiału identyfikuje materiały funkcjonalne.", "Tak, ale tylko dla materiałów poddanych działaniu pola elektrycznego.", "Tak, ale tylko dla materiałów poddanych działaniu pola mechanicznego."], correct: 0 },
            { id: 59, q: "Stan naprężenia można rozłożyć na dwa stany podstawowe:", type: "normal", answers: ["stan hydrostatyczny oraz czyste ścinanie", "stan naprężeń głównych oraz stan naprężeń stycznych", "stan naprężeń średnich oraz stan naprężeń oktaedrycznych"], correct: 0 },
            { id: 60, q: "Zanieczyszczenia w stali takie jak siarka i fosfor:", type: "normal", answers: ["nie wpływają na skrawalność", "pogarszają skrawalność", "polepszają skrawalność"], correct: 2 },
            { id: 61, q: "Podaj, który z procesów obróbki powierzchniowej wymaga koniecznie stosowania próżni:", type: "normal", answers: ["hartowanie laserowe", "hartowanie plazmowe", "hartowanie elektronowe"], correct: 1 },
            { id: 62, q: "Która z wymienionych powłok na wyrobie stalowym ma charakter powłoki anodowej?", type: "normal", answers: ["Zn", "Sn", "Cu"], correct: 0 },
            { id: 63, q: "Który z wymienionych materiałów jest najwłaściwszym materiałem na pojemniki do przechowywania ciekłych gazów?", type: "normal", answers: ["Stop aluminium", "Stal niskostopowa o podwyższonej wytrzymałości", "Chromowa stal ferrytyczna"], correct: 1 },
            { id: 64, q: "Powstawanie stopów eutektycznych zachodzi, gdy:", type: "normal", answers: ["nieograniczona rozpuszczalność, podobna temp topnienia", "nieograniczona rozpuszczalność, różne temp topnienia", "ograniczona rozpuszczalność, różne temp topnienia", "ograniczona rozpuszczalność, podobna temp topnienia"], correct: 3 },
            { id: 65, q: "Stal niskonapięciowa (niskowęglowa) jest miększa, bo: - ma więcej ferrytu, - ma mniej węgla", type: "normal", answers: ["tak tak", "nie tak", "nie nie", "tak nie"], correct: 0 },
            { id: 66, q: "Na czym polega obróbka cieplna stali?", type: "ai", answers: ["Chodzi o to żeby rozpuścić niepożądane fazy", "Na mechanicznym usuwaniu zgorzeliny", "Na powierzchniowym utwardzaniu przez zgniot"], correct: 0 },
            { id: 67, q: "Spiekanie to:", type: "ai", answers: ["proces samorzutny i nieodwracalny", "proces wymuszony i w pełni odwracalny", "topnienie osnowy w łuku elektrycznym"], correct: 0 },
            { id: 68, q: "Dla kompozytów o osnowie ceramicznej najbardziej typowy napełniacz to:", type: "ai", answers: ["Ziarnisty", "Włóknisty ciągły", "Warstwowy (laminat)"], correct: 0 },
            { id: 69, q: "Materiały ogniotrwałe magnezjanowe należą do grupy:", type: "normal", answers: ["kwaśne", "zasadowe", "obojętne", "specjalne"], correct: 1 },
            { id: 70, q: "Jaki jest przeciętny współczynnik KIc dla ceramik?", type: "normal", answers: ["0,1-1 MPa·m^(1/2)", "1-10", "20-40", "40-100"], correct: 1 },
            { id: 71, q: "W jakiej próbie ceramika wykazuje największą wytrzymałość?", type: "ai", answers: ["Na ściskanie *(rozciąganie < zginanie << ściskanie)", "Na rozciąganie", "Na skręcanie"], correct: 0 },
            { id: 72, q: "Przy jakim napełniaczu kompozyt uzyskuje własności izotropowe?", type: "normal", answers: ["Dyspersyjny", "cząstki", "krótkie włókna", "długie włókna"], correct: 1 },
            { id: 73, q: "Któremu z procesów towarzyszy wydzielenie małocząsteczkowych produktów ubocznych?", type: "normal", answers: ["polimeryzacja", "poliaddycja", "polikondensacja", "kopolimeryzacja"], correct: 2 },
            { id: 74, q: "Duroplasty:", type: "ai", answers: ["Obrabialne cieplnie jeden raz, nie można obrabiać po utwardzeniu", "Mogą być wielokrotnie topione i formowane", "Są elastyczne w każdej temperaturze"], correct: 0 },
            { id: 75, q: "Jaką charakterystyczną właściwość wykazuje materiał elektrostrykcyjny?", type: "ai", answers: ["Mechaniczne odkształcenia pod wpływem napięcia elektrycznego", "Wytwarzanie napięcia pod wpływem ściskania", "Zmiana koloru pod wpływem prądu"], correct: 0 },
            { id: 76, q: "Na czym polega efekt Barusa?", type: "ai", answers: ["zmiana kształtu materiału po opuszczeniu dyszy wtryskowej.", "gwałtowny wzrost twardości przy szybkim chłodzeniu", "zanik magnetyzmu w temperaturze Curie"], correct: 0 },
            { id: 77, q: "Co posiada największy wpływ na moduł Younga metali?", type: "ai", answers: ["Wiązania metaliczne, moduł większy im większa energia wiązania.", "Wielkość ziarna po wyżarzaniu", "Ilość defektów punktowych w sieci"], correct: 0 },
            { id: 78, q: "Dlaczego stal niskowęglowa ma większą ciągliwość niż średnio-węglowa?", type: "ai", answers: ["Bo jest mniejsza zawartość węgla. Wraz ze wzrostem węgla w stali ubywa miękkiego i plastycznego ferrytu a przybywa twardego cementytu.", "Ponieważ ferryt ulega przemianie martenzytycznej", "Ze względu na obecność grafitu sferoidalnego"], correct: 0 },
            { id: 79, q: "Wytrzymałość mechaniczna ceramiki wynosi:", type: "ai", answers: ["10^2 MPa – wytrzymałość rzeczywista, Teoretyczna – 10-102 GPa.", "10-20 MPa - wytrzymałość rzeczywista, Teoretyczna - 1 GPa", "Powyżej 5000 MPa w każdej próbie rozciągania"], correct: 0 },
            { id: 80, q: "Dopasuj poprawnie defekty mikrostruktury (Wakancje, Dyslokacje, Granice ziarn, Cząstki):", type: "ai", answers: ["Punktowe->Wakancje, Liniowe->Dyslokacje, Powierzchniowe->Granice ziarn, Objętościowe->Cząstki", "Punktowe->Dyslokacje, Liniowe->Wakancje, Powierzchniowe->Cząstki, Objętościowe->Granice ziarn", "Punktowe->Granice ziarn, Liniowe->Cząstki, Powierzchniowe->Wakancje, Objętościowe->Dyslokacje"], correct: 0 },
            { id: 81, q: "Współczynnik załamania światła rdzenia światłowodu:", type: "ai", answers: ["Rdzeń musi mieć większy współczynnik załamania światła(n2) od materiału otaczającego(n1).", "Rdzeń musi mieć mniejszy współczynnik załamania światła od płaszcza", "Musi być równy współczynnikowi załamania światła powietrza"], correct: 0 },
            { id: 82, q: "Do obróbki jakich materiałów używane są ściernice twarde?", type: "ai", answers: ["Do szlifowania materiałów miękkich.", "Do szlifowania węglików spiekanych", "Do obróbki wykończeniowej diamentów"], correct: 0 },
            { id: 83, q: "Wskaż, który kompozyt poliestrowy zbrojony włóknami posiada najwyższy moduł sprężystości:", type: "ai", answers: ["CFRP", "GFRP", "AFRP (Kevlar)"], correct: 0 },
            { id: 84, q: "Który z wymienionych rodzajów kompozytów posiada szczególnie dużą zdolność pochłaniania energii uderzenia?", type: "ai", answers: ["Kevlar", "Kompozyt szklany", "Kompozyt borowy"], correct: 0 },
            { id: 85, q: "Czym zajmuje się bionika (biomimetyka)?", type: "ai", answers: ["nauka badająca budowę i zasady działania organizmów oraz ich adaptowanie w technice", "hodowlą sztucznych tkanek na osnowie polimerowej", "badaniem korozji mikrobiologicznej stali"], correct: 0 },
            { id: 86, q: "Parametry decydujące o przebiegu procesu krzepnięcia:", type: "ai", answers: ["Odkształcenie, spiekanie, przechłodzenie", "Prędkość nagrzewania i czas wytrzymania", "Ciśnienie atmosferyczne i wilgotność powietrza"], correct: 0 },
            { id: 87, q: "Mer to podstawowa cząsteczka:", type: "ai", answers: ["polimeru", "kryształu martenzytu", "cieczy eutektycznej"], correct: 0 },
            { id: 88, q: "Brązy cynowe odlewnicze charakteryzuje:", type: "ai", answers: ["mały skurcz odlewniczy", "bardzo duża skłonność do pękania na gorąco", "brak rozpuszczalności cyny w miedzi"], correct: 0 },
            { id: 89, q: "W podeutektycznym stopie układu dwuskładnikowego z ograniczoną rozpuszczalnością B w A i brakiem rozpuszczalności A w B mikrostruktura składa się z:", type: "ai", answers: ["pierwotnych kryształów B i eutektyki alfa w B", "czystego cementytu i perlitu", "kryształów martenzytu i austenitu szczątkowego"], correct: 0 },
            { id: 90, q: "Podczas wyżarzania rekrystalizującego zgniot krytyczny powoduje:", type: "ai", answers: ["utworzenie struktury gruboziarnistej", "całkowite usunięcie dyslokacji bez zmiany ziarna", "pękanie miedzykonstrukcyjne materiału"], correct: 0 },
            { id: 91, q: "W których żeliwach nie występują wydzielenia grafitu:", type: "ai", answers: ["W białych", "W szarych", "W sferoidalnych"], correct: 0 },
            { id: 92, q: "Zdecydowana większość metali krystalizuje w następujących układach krystalograficznych:", type: "ai", answers: ["Regularnym lub heksagonalnym", "Trójskośnym lub jednoskośnym", "Rombowym lub tetragonalnym"], correct: 0 },
            { id: 93, q: "Wskaż prawidłowo zapisany wskaźnik kierunku krystalograficznego:", type: "ai", answers: ["[101]", "(101)", "{101}"], correct: 0 },
            { id: 94, q: "Elementy przeznaczone do azotowania hartuje się:", type: "ai", answers: ["przed azotowaniem", "w trakcie procesu azotowania", "bezpośrednio po azotowaniu"], correct: 0 },
            { id: 95, q: "Korozji międzykrystalicznej w stali zapobiegamy przez:", type: "ai", answers: ["wprowadzenie niklu (i tytanu) albo zmniejszenie zawartości węgla", "zwiększenie zawartości fosforu w stopie", "hartowanie z niskim odpuszczaniem"], correct: 0 },
            { id: 96, q: "Zawartość cynku w jednofazowym mosiądzu mieści się w zakresie:", type: "ai", answers: ["do koło 12%", "od 30% do 45%", "zawsze powyżej 50%"], correct: 0 },
            { id: 97, q: "Umocnienie wydzieleniowe składa się z dwóch następujących bezpośrednio po sobie procesów:", type: "ai", answers: ["przesycania i starzenia", "hartowania i odpuszczania", "wyżarzania i zgniotu"], correct: 0 },
            { id: 98, q: "Obróbka powierzchniowa zmienia:", type: "ai", answers: ["chropowatość i odporność na korozję", "moduł Younga całego rdzenia", "skład chemiczny w osi symetrii próbki"], correct: 0 },
            { id: 99, q: "Stale szybkotnące to stale:", type: "ai", answers: ["Wysokowęglowe", "Niskowęglowe", "Automatowe bezwęglowe"], correct: 0 },
            { id: 100, q: "Który z poniższych wzorów opisuje regułę faz Gibbsa przy stałym ciśnieniu?", type: "ai", answers: ["s = n - f + 1", "s = n - f + 2", "s = n - f"], correct: 0 },
            { id: 101, q: "Spośród faz i składników strukturalnych występujących na układzie Fe-Fe3C najlepsze własności plastyczne posiada:", type: "ai", answers: ["Ferryt", "Cementyt", "Ledeburyt"], correct: 0 },
            { id: 102, q: "Jaka jest temperatura przemiany alotropowej żelaza alfa w żelazo gamma:", type: "ai", answers: ["912 stopni C", "727 stopni C", "1147 stopni C"], correct: 0 },
            { id: 103, q: "Mikrostruktura stali niestopowej konstrukcyjnej w stanie równowagi (wyżarzonym) składa się z:", type: "ai", answers: ["ferrytu i perlitu", "martenzytu i austenitu", "bainitu i cementytu"], correct: 0 },
            { id: 104, q: "Cementyt trzeciorzędowy wydziela się w stalach podczas chłodzenia:", type: "ai", answers: ["z ferrytu na skutek malejącej rozpuszczalności węgla", "z austenitu podczas przemiany martenzytycznej", "bezpośrednio z cieczy przy temp. 1147°C"], correct: 0 },
            { id: 105, q: "Pierwiastki austenitotwórcze to:", type: "ai", answers: ["mangan i nikiel", "chrom i wolfram", "tytan i wanad"], correct: 0 },
            { id: 106, q: "Fazy Lavesa są to:", type: "ai", answers: ["fazy międzymetaliczne", "roztwory graniczne", "odmiany alotropowe ferrytu"], correct: 0 },
            { id: 107, q: "Do defektów struktury krystalicznej należą:", type: "ai", answers: ["dyslokacje, błędy ułożenia, granice ziarn", "wtrącenia niemetaliczne, pęcherze gazowe", "pęknięcia zmęczeniowe, zgorzelina"], correct: 0 }
        ];

        let currentMode = 'normal';
        let isShuffle = false;
        let wrongAnswersList = JSON.parse(localStorage.getItem('wrongAnswersList')) || [];
        
        let activeQuestions = [...database];
        let currentQuestionIndex = 0;
        let hasAnswered = false;

        function initQuiz() {
            updateReviewCountDisplay();
            setupQuestions();
            showQuestion();
        }

        function setupQuestions() {
            const searchQuery = document.getElementById('search-input').value.toLowerCase().trim();

            if (currentMode === 'learn') {
                activeQuestions = database.filter(q => wrongAnswersList.includes(q.id));
            } else {
                activeQuestions = [...database];
            }

            if (searchQuery !== '') {
                activeQuestions = activeQuestions.filter(q => q.q.toLowerCase().includes(searchQuery));
            }

            if (isShuffle) {
                activeQuestions.sort(() => Math.random() - 0.5);
            } else {
                activeQuestions.sort((a, b) => a.id - b.id);
            }
            
            currentQuestionIndex = 0;
            populateJumpSelect();
        }

        function populateJumpSelect() {
            const select = document.getElementById('jump-select');
            select.innerHTML = '<option value="">Skocz do pytania...</option>';
            
            activeQuestions.forEach((q, index) => {
                const opt = document.createElement('option');
                opt.value = index;
                opt.innerText = `Pytanie ${q.id}: ${q.q.substring(0, 40)}...`;
                select.appendChild(opt);
            });
        }

        function handleJumpToQuestion() {
            const select = document.getElementById('jump-select');
            if (select.value !== "") {
                currentQuestionIndex = parseInt(select.value, 10);
                showQuestion();
            }
        }

        function handleSearch() {
            setupQuestions();
            showQuestion();
        }

        function setMode(mode) {
            currentMode = mode;
            document.getElementById('mode-normal').classList.toggle('active', mode === 'normal');
            document.getElementById('mode-learn').classList.toggle('active', mode === 'learn');
            setupQuestions();
            showQuestion();
        }

        function toggleShuffle() {
            isShuffle = !isShuffle;
            document.getElementById('shuffle-btn').innerText = `Losowe pytania: ${isShuffle ? 'WŁ' : 'WYŁ'}`;
            document.getElementById('shuffle-btn').classList.toggle('active', isShuffle);
            setupQuestions();
            showQuestion();
        }

        function updateReviewCountDisplay() {
            document.getElementById('review-count').innerText = wrongAnswersList.length;
        }

        function showQuestion() {
            hasAnswered = false;
            document.getElementById('next-btn').disabled = true;
            
            const quizArea = document.getElementById('quiz-area');
            const totalCountSpan = document.getElementById('total-count');
            const currentIndexSpan = document.getElementById('current-index');
            
            document.getElementById('jump-select').value = (activeQuestions.length > 0 && currentQuestionIndex < activeQuestions.length) ? currentQuestionIndex : "";

            if (activeQuestions.length === 0) {
                const isSearching = document.getElementById('search-input').value.trim() !== '';
                let msg = currentMode === 'learn' ? 'Brak pytań do powtórki!' : 'Brak pytań.';
                if (isSearching) msg = 'Nie znaleziono pytań pasujących do frazy.';
                
                quizArea.innerHTML = `<div class="empty-state"><h3>${msg}</h3></div>`;
                currentIndexSpan.innerText = "0";
                totalCountSpan.innerText = "0";
                return;
            }

            if (!document.getElementById('question-id')) {
                quizArea.innerHTML = `
                    <div class="question-box">
                        <div class="q-number" id="question-id"></div>
                        <div class="q-text" id="question-text"></div>
                    </div>
                    <div class="options-list" id="options-container"></div>
                    <div class="actions"><button id="next-btn" onclick="nextQuestion()" disabled>Następne pytanie &rarr;</button></div>
                `;
            }

            const qData = activeQuestions[currentQuestionIndex];
            
            currentIndexSpan.innerText = currentQuestionIndex + 1;
            totalCountSpan.innerText = activeQuestions.length;

            document.getElementById('question-id').innerText = `Pytanie ${qData.id}`;
            
            let qTextElement = document.getElementById('question-text');
            qTextElement.innerText = qData.q;
            
            if (qData.type === 'ai') {
                const badge = document.createElement('span');
                badge.className = 'ai-badge';
                badge.innerText = '* [AI]';
                qTextElement.appendChild(badge);
            }

            const container = document.getElementById('options-container');
            container.innerHTML = '';

            qData.answers.forEach((ans, index) => {
                const optDiv = document.createElement('div');
                optDiv.className = 'option';
                optDiv.innerText = ans;
                
                // USUNIĘTE: Brak modyfikacji stylu fontWeight na tym etapie

                optDiv.onclick = () => selectOption(index, optDiv);
                container.appendChild(optDiv);
            });
        }

        function selectOption(selectedIndex, optElement) {
            if (hasAnswered) return;
            hasAnswered = true;

            const qData = activeQuestions[currentQuestionIndex];
            const container = document.getElementById('options-container');
            const options = container.children;

            if (selectedIndex === qData.correct) {
                optElement.classList.add('correct');
                if (currentMode === 'learn') {
                    wrongAnswersList = wrongAnswersList.filter(id => id !== qData.id);
                    localStorage.setItem('wrongAnswersList', JSON.stringify(wrongAnswersList));
                    updateReviewCountDisplay();
                }
            } else {
                optElement.classList.add('wrong');
                options[qData.correct].classList.add('correct');
                
                if (!wrongAnswersList.includes(qData.id)) {
                    wrongAnswersList.push(qData.id);
                    localStorage.setItem('wrongAnswersList', JSON.stringify(wrongAnswersList));
                    updateReviewCountDisplay();
                }
            }

            for (let i = 0; i < options.length; i++) {
                options[i].classList.add('disabled');
            }

            document.getElementById('next-btn').disabled = false;
        }

        function nextQuestion() {
            currentQuestionIndex++;
            if (currentQuestionIndex < activeQuestions.length) {
                showQuestion();
            } else {
                alert("Koniec pytań w tym zestawie!");
                setupQuestions();
                showQuestion();
            }
        }

        window.onload = initQuiz;
    </script>
</body>
</html>
