<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Quiz de Formação de Palavras - Português</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0/css/all.min.css" rel="stylesheet">
    <style>
        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background-color: #f3f4f6;
        }
        .option-label {
            transition: all 0.2s;
        }
        .option-input:checked + .option-label {
            background-color: #e5e7eb;
            border-color: #4b5563;
        }
        .correct-answer {
            background-color: #d1fae5 !important;
            border-color: #10b981 !important;
            color: #065f46 !important;
        }
        .wrong-answer {
            background-color: #fee2e2 !important;
            border-color: #ef4444 !important;
            color: #991b1b !important;
        }
        .question-card {
            box-shadow: 0 4px 6px -1px rgba(0, 0, 0, 0.1), 0 2px 4px -1px rgba(0, 0, 0, 0.06);
        }
    </style>
</head>
<body class="pb-20">

    <!-- Header / Navbar -->
    <header class="bg-indigo-700 text-white shadow-lg sticky top-0 z-50">
        <div class="container mx-auto px-4 py-3 flex justify-between items-center">
            <div>
                <h1 class="text-xl font-bold md:text-2xl">Formação de Palavras</h1>
                <p class="text-xs md:text-sm text-indigo-200">Banco de Questões Militares</p>
            </div>
            <div class="text-right">
                <div class="text-xs md:text-sm font-semibold mb-1">
                    <span class="text-green-300" title="Acertos"><i class="fas fa-check mr-1"></i><span id="score-display">0</span></span>
                    <span class="mx-2 text-indigo-400">|</span>
                    <span class="text-red-300" title="Erros"><i class="fas fa-times mr-1"></i><span id="wrong-display">0</span></span>
                    <span class="text-indigo-300 ml-1 text-xs">/ <span id="total-questions-display">0</span></span>
                </div>
                <div class="w-32 md:w-48 bg-indigo-900 rounded-full h-3 flex overflow-hidden bg-opacity-50">
                    <div id="progress-bar-correct" class="bg-green-500 h-full transition-all duration-500" style="width: 0%"></div>
                    <div id="progress-bar-wrong" class="bg-red-500 h-full transition-all duration-500" style="width: 0%"></div>
                </div>
            </div>
        </div>
    </header>

    <!-- Main Content -->
    <main class="container mx-auto px-4 py-8 max-w-4xl" id="quiz-container">
        <!-- Questions will be injected here by JS -->
        <div class="text-center py-10">
            <i class="fas fa-spinner fa-spin text-4xl text-indigo-600"></i>
            <p class="mt-4 text-gray-600">Carregando questões...</p>
        </div>
    </main>

    <!-- Floating Action Button for Reset -->
    <button onclick="resetQuiz()" class="fixed bottom-6 right-6 bg-red-600 text-white p-4 rounded-full shadow-lg hover:bg-red-700 transition duration-300 z-40" title="Reiniciar Quiz">
        <i class="fas fa-redo-alt"></i>
    </button>

    <script>
        // Data Structure
        const quizData = [
            {
                id: 1,
                source: "(CFN 2020)",
                text: "As palavras continho, barrigudinho e engraçadinho, encontradas no texto 2, são derivações dos vocábulos conto, barriga e graça, respectivamente. De forma semelhante, as palavras destacadas nas frases abaixo também sofreram algum tipo de derivação. Assim, analise-as e marque a opção que indica o tipo de derivação correspondente a cada uma.<br><br>I – Ela disse um <b>nunca</b> definitivo.<br>II – A <b>pesca</b> foi abundante.<br>III – Todos estavam escondidos no <b>subterrâneo</b>.",
                options: [
                    "a) Parassintética – imprópria – regressiva.",
                    "b) Parassintética – regressiva – imprópria.",
                    "c) Regressiva – imprópria – parassintética.",
                    "d) Imprópria – parassintética – regressiva.",
                    "e) Imprópria – regressiva – parassintética."
                ],
                correct: 4 // e
            },
            {
                id: 2,
                source: "(CFN 2021)",
                text: "Em: “A única diferença é um contorno, uma paragem, uma cor que <b>destinge</b>[...]” (linhas 11 e 12), como se deu a formação da palavra em destaque?",
                options: [
                    "a) Por derivação sufixal.",
                    "b) Por derivação parassintética.",
                    "c) Por derivação prefixal.",
                    "d) Por derivação regressiva.",
                    "e) Por derivação imprópria."
                ],
                correct: 2 // c
            },
            {
                id: 3,
                source: "(CFN 2022)",
                text: "Leia o trecho: “Até o século XVII, a criança era vista como estorvo, desgraça, um fardo insuportável para a família.”(linhas 2 a 4). Quanto à formação de palavras, a palavra em destaque (<b>desgraça</b>) foi formada por qual tipo de derivação?",
                options: [
                    "a) Imprópria.",
                    "b) Parassintética.",
                    "c) Regressiva.",
                    "d) Sufixal.",
                    "e) Prefixal."
                ],
                correct: 4 // e
            },
            {
                id: 4,
                source: "(Colégio Naval 2018)",
                text: "Em \"Não transformar o lenço em pomba, mas usar o lenço para dar o recado, um <b>lençocorreio</b>\". o autor utiliza-se da formação de palavras por meio do processo de:",
                options: [
                    "a) derivação prefixal.",
                    "b) derivação sufixal.",
                    "c) derivação parassintética.",
                    "d) Justaposição.",
                    "e) Aglutinação."
                ],
                correct: 3 // d
            },
            {
                id: 5,
                context: "<b>Texto para a questão 5: ESCRAVIDÃO CONTEMPORÂNEA</b><br>O trabalho escravo de hoje pouco lembra aquele de outrora – com trabalhadores acorrentados ou castigados sob desmandos vários. Mas nem por isso ele é menos cruel. Senzalas foram substituídas por barracos imundos. Correntes foram trocadas por regimes inescapáveis de servidão. O próprio sítio do MPT - Ministério Público do Trabalho – traz uma página especialmente dedicada ao assunto; “ trabalho forçado, servidão por dívidas, jornadas exaustivas ou condições degradantes como alojamento precário, água não potável, alimentação inadequada, falta de registro, maus-tratos e violência” são alguns dos itens elencados pelo órgão.<br><i>(Kugler, Henrique: Ciência Hoje, número 309/vol 52/novembro de 2013, pág.37)</i>",
                source: "(EPCAR 2014)",
                text: "Marque a opção que traz uma análise correta.",
                options: [
                    "a) Nas palavras “página” (l. 6) e “precário” (l. 9), o uso do acento gráfico pode ser justificado pela mesma regra.",
                    "b) “itens elencados” (l. 11) significam “itens ignorados.”",
                    "c) “desmandos vários.” (l. 2 e 3) significam “intensos desmandos”.",
                    "d) “maus-tratos e violência”, (l. 10 e 11), são palavras formadas, respectivamente, por aglutinação e sufixação."
                ],
                correct: 0 // a
            },
            {
                id: 6,
                source: "(EsSA 2011)",
                text: "Há um caso típico de palavra formada por composição em",
                options: [
                    "a) aguardente.",
                    "b) pesca.",
                    "c) amanhecer.",
                    "d) perigosamente.",
                    "e) repatriar."
                ],
                correct: 0 // a
            },
            {
                id: 7,
                source: "(EsSA 2012)",
                text: "São formadas por derivação prefixal, sufixal e parassintética, respectivamente, a sequência",
                options: [
                    "a) abdicar, pernoite, descer.",
                    "b) superpor, forense, amanhecer.",
                    "c) suavisar, dispneia, ensurdecer.",
                    "d) embainhar, sinfonia, bondosamente.",
                    "e) abotoar, ponteiro, intravenoso."
                ],
                correct: 1 // b
            },
            {
                id: 8,
                source: "(EsSA 2014)",
                text: "Marque a opção cuja palavra apresente um prefixo com o mesmo significado do prefixo destacado na palavra “<b>in</b>verdades\":",
                options: [
                    "a) afônico",
                    "b) iminente",
                    "c) encéfalo",
                    "d) anteposto",
                    "e) introvertido"
                ],
                correct: 0 // a
            },
            {
                id: 9,
                source: "(EsSA 2015)",
                text: "Assinale a palavra abaixo cujo prefixo apresente o mesmo valor semântico do prefixo componente de",
                options: [
                    "a) Antibiótico.",
                    "b) Importação.",
                    "c) Insatisfeito.",
                    "d) Adjacência.",
                    "e) Antebraço"
                ],
                correct: 2 // c
            },
            {
                id: 10,
                source: "(EsSA 2016)",
                text: "Marque a única alternativa em que as palavras não se formam pelo processo de composição.",
                options: [
                    "a) beija-flor; pernalta",
                    "b) amanhecer; desalmado",
                    "c) embora; segunda-feira",
                    "d) pé-de-meia; aguardente",
                    "e) tira-teima; madrepérola"
                ],
                correct: 1 // b
            },
            {
                id: 11,
                source: "(EsSA 2017)",
                text: "Em “Você que ama velocidade, agora pode chegar ao <b>finalmente</b>”, a palavra sublinhada foi formada por:",
                options: [
                    "a) derivação prefixal e sufixal",
                    "b) derivação imprópria",
                    "c) derivação regressiva",
                    "d) derivação prefixal",
                    "e) composição por justaposição"
                ],
                correct: 1 // b
            },
            {
                id: 12,
                source: "(EsSA 2018)",
                text: "Quanto ao processo de formação de palavras, escreva V ou F conforme sejam verdadeiras ou falsas as assertivas abaixo. Logo, assinale a alternativa que tenha a sequência correta.<br><br>I bicicleta, automóvel, sociologia – Hibridismo.<br>II embora, agricultura, maldizente – Composição.<br>III ponteira, sozinho, bombeiro - Derivação.",
                options: [
                    "a) F, F, V.",
                    "b) V, V, F.",
                    "c) V, F, F.",
                    "d) F, V, V.",
                    "e) V, V, V"
                ],
                correct: 4 // e
            },
            {
                id: 13,
                source: "(EsSA 2022)",
                text: "O único caso em que a palavra é formada por composição ocorre em:",
                options: [
                    "a) amparo",
                    "b) ensurdecer",
                    "c) gotícula",
                    "d) pernalta",
                    "e) desfazer"
                ],
                correct: 3 // d
            },
            {
                id: 14,
                source: "(EEAr 2. 2016)",
                text: "Considerando que a palavra natremia significa a presença de sódio no sangue, é correto afirmar que a palavra <b>hiponatremia</b> é formada por",
                options: [
                    "a) aglutinação.",
                    "b) justaposição.",
                    "c) derivação prefixal.",
                    "d) derivação parassintética."
                ],
                correct: 2 // c
            },
            {
                id: 15,
                source: "(EEAr 1. 2017)",
                text: "Assinale a alternativa incorreta quanto à formação da palavra em destaque.",
                options: [
                    "a) A vida só é possível / <b>reinventada</b>. (Cecília Meireles) – derivação parassintética",
                    "b) O amor deixará de variar, se for firme, mas não deixará de <b>tresvariar</b>, se é amor. (Pe. Antônio Vieira) – derivação prefixal",
                    "c) O senhor tolere, isto é o <b>sertão</b> (...) Lugar sertão se divulga: é onde os pastos carecem de fechos. (Guimarães Rosa) – derivação imprópria",
                    "d) Mas o livro é enfadonho, cheira a sepulcro, traz certa contração <b>cadavérica</b>; vício grave, e aliás ínfimo (...) (Machado de Assis) – derivação sufixal"
                ],
                correct: 0 // a
            },
            {
                id: 16,
                source: "(EEAr 2. 2017)",
                text: "O vocábulo <b>alistar</b> segue o mesmo processo de formação de palavras presente em",
                options: [
                    "a) descarregar.",
                    "b) empalidecer.",
                    "c) achatamento.",
                    "d) desligamento."
                ],
                correct: 1 // b
            },
            {
                id: 17,
                source: "(EEAr 1. 2019)",
                text: "Considerando o processo de formação de palavras, relacione as duas colunas e assinale a alternativa que apresenta a sequência correta.<br><br>1 – prefixação e sufixação ( ) desenlace<br>2 – parassíntese ( ) indeterminadamente<br>3 – prefixação ( ) ensaboar<br>4 – aglutinação ( ) petróleo",
                options: [
                    "a) 3 – 2 – 4 – 1",
                    "b) 3 – 1 – 2 – 4",
                    "c) 2 – 3 – 1 – 4",
                    "d) 4 – 2 – 1 – 3"
                ],
                correct: 1 // b
            },
            {
                id: 18,
                source: "(EEAr 2. 2019)",
                text: "Leia:<br>“O meu <b>amanhecer</b> tem o <b>cantar</b> do galo<br>O cheiro do mato com gota de orvalho...”<br>As palavras destacadas nos versos acima são formadas, respectivamente, por derivação",
                options: [
                    "a) parassintética, imprópria, parassintética.",
                    "b) imprópria, regressiva, parassintética.",
                    "c) imprópria, imprópria, regressiva.",
                    "d) regressiva, regressiva, imprópria."
                ],
                correct: 2 // c
            },
            {
                id: 19,
                source: "(EEAr 1. 2020)",
                text: "Assinale a alternativa que classifica, correta e respectivamente, o processo de formação das palavras destacadas nas frases abaixo.<br><br>1 – “Ia tomar sol, esquentar o corpo gigantesco que agora se <b>dobrava</b> em dois...”<br>2 – “Gostava tanto de ver o <b>florir</b> e o carregar do cacau...”<br>3 – “O rapaz avistou um vulto e, <b>inconsequentemente</b>, soltou um grito, acordando a fera.”",
                options: [
                    "a) sufixação - derivação regressiva - parassíntese",
                    "b) prefixação - derivação regressiva - parassíntese",
                    "c) sufixação - derivação imprópria - prefixação e sufixação",
                    "d) prefixação - derivação imprópria - prefixação e sufixação"
                ],
                correct: 2 // c
            },
            {
                id: 20,
                source: "(EEAr 1. 2021)",
                text: "Assinale a alternativa que apresenta o correto significado da palavra, considerando-se o prefixo destacado.",
                options: [
                    "a) <b>infra</b>mencionado: mencionado acima",
                    "b) <b>ante</b>clássico: contrário ao clássico",
                    "c) <b>intro</b>spectivo: voltado para fora",
                    "d) <b>pos</b>tergar: deixar para depois"
                ],
                correct: 3 // d
            },
            {
                id: 21,
                source: "(EEAr 1. 2021)",
                text: "Assinale a alternativa em que o sufixo em destaque indica forma de proceder.",
                options: [
                    "a) Que falta fazem as boas notícias que enlevam por demonstrações de hero<b>ísmo</b>!",
                    "b) Os investigadores se depararam com um cenário dant<b>esco</b> ao adentrar o local do crime.",
                    "c) Era evidente o magnetismo que exalava o casal com seus ousados e perfeitos passos de dan<b>ça</b>.",
                    "d) Ninguém esperava uma reação tão imprópria, tão infan<b>til</b>, tão surpreendente para um homem barbado daquele."
                ],
                correct: 0 // a
            },
            {
                id: 22,
                source: "(EEAr 1. 2022)",
                text: "Leia:<br>“E todo aquele <b>retintim</b> de ferramentas, e o <b>martelar</b> da forja, e o coro dos que lá em cima brocavam a rocha para lançar-lhe fogo, e a surda zoada de longe, que vinha do cortiço, como de uma aldeia alarmada; tudo dava a ideia de uma atividade feroz, de uma luta de vingança e de ódio.” (Aluísio Azevedo)<br>As palavras em destaque no texto acima são formadas, respectivamente, pelos processos de",
                options: [
                    "a) onomatopeia, derivação imprópria, derivação sufixal.",
                    "b) onomatopeia, derivação regressiva, derivação prefixal.",
                    "c) derivação prefixal, derivação sufixal, derivação parassintética.",
                    "d) derivação sufixal, derivação imprópria, derivação prefixal e sufixal."
                ],
                correct: 0 // a
            },
            {
                id: 23,
                source: "(EEAr 2. 2022)",
                text: "Leia o texto abaixo e assinale a alternativa correta com relação aos processos de formação de palavras.<br>“Ele é um homem ainda moço, de 30 anos presumíveis, magro, de estatura média. Seu olhar é morto, contemplativo. Suas feições transmitem bondade, tolerância e há em seu rosto um quê de instabilidade. Seus gestos são lentos, preguiçosos, bem como sua maneira de falar.” (Dias Gomes)",
                options: [
                    "a) Instabilidade é formada por derivação prefixal.",
                    "b) Olhar e quê têm o mesmo processo de formação.",
                    "c) Magro, lento e preguiçoso são palavras primitivas.",
                    "d) Há três adjetivos provenientes de verbo por derivação sufixal."
                ],
                correct: 1 // b
            },
            {
                id: 24,
                source: "(EAGS 2011)",
                text: "Leia:<br>O sol <b>amarelado</b><br>Apontou no <b>descampado</b><br>E no <b>corre-corre</b> do dia<br>Nem foi admirado<br>As palavras em destaque nos versos acima foram formadas, respectivamente, pelos processos de",
                options: [
                    "a) prefixação, aglutinação e justaposição.",
                    "b) sufixação, derivação parassintética e aglutinação.",
                    "c) derivação parassintética, justaposição e prefixação.",
                    "d) sufixação, derivação parassintética e justaposição."
                ],
                correct: 3 // d
            },
            {
                id: 25,
                source: "(EAGS 2012)",
                text: "Em qual das alternativas a palavra destacada é formada por prefixação?",
                options: [
                    "a) Sedentos, aqueles pobres homens caminhavam pela areia quente do deserto.",
                    "b) O chefe, embora tivesse um semblante muito sisudo, possuía um enorme coração.",
                    "c) A <b>desocupação</b> daquele local exigiu do Prefeito e do Governador atitudes desumanas.",
                    "d) Dizem as pesquisas recentes que mais de 98% da plantação de caju encontra-se no Nordeste, cujo solo é arenoso."
                ],
                correct: 2 // c
            },
            {
                id: 26,
                source: "(EAGS 2013)",
                text: "Leia:<br>As <b>inundações</b> provocadas pelas fortes chuvas foram o assunto do <b>debate</b> dos candidatos à prefeitura.<br>Em qual alternativa as palavras são formadas, respectivamente, pelo mesmo processo encontrado nas palavras destacadas no texto acima?",
                options: [
                    "a) inútil – desalmado",
                    "b) manhoso – disputa",
                    "c) choro – desordem",
                    "d) empobrecer – erro"
                ],
                correct: 1 // b
            },
            {
                id: 27,
                source: "(EAGS 2014)",
                text: "Assinale a alternativa em que a palavra é formada pelo processo de composição por aglutinação.",
                options: [
                    "a) finalmente",
                    "b) semicírculo",
                    "c) vinagre",
                    "d) girassol"
                ],
                correct: 2 // c
            },
            {
                id: 28,
                source: "(EAGS 2015)",
                text: "Em qual alternativa todas as palavras são formadas pelo processo de derivação parassintética?",
                options: [
                    "a) desocupar, emudece",
                    "b) liberalismo, tendinite",
                    "c) incoerente, refeitório",
                    "d) alinhar, abreviar"
                ],
                correct: 3 // d
            },
            {
                id: 29,
                source: "(EAGS 2017)",
                text: "Leia:<br>I. O <b>alcoolismo</b> é um dos fatores que contribui para a violência contra crianças e mulheres.<br>II. Nos EUA, os gastos com a violência doméstica entre casais <b>ultrapassa</b> 5,8 bilhões de dólares anuais.<br>III. O <b>olhar</b> dos estrangeiros sobre o Brasil vai além das belezas naturais; o turismo sexual é um forte atrativo do país.<br>IV. As denúncias de turismo sexual precisam ser feitas, a fim de <b>enfraquecer</b> esse sistema doente.<br>O processo de formação das palavras destacadas acima é, respectivamente, derivação",
                options: [
                    "a) sufixal / prefixal / regressiva / prefixal e sufixal.",
                    "b) sufixal / prefixal / imprópria / parassintética.",
                    "c) prefixal / regressiva / imprópria / sufixal.",
                    "d) prefixal / sufixal / regressiva / prefixal."
                ],
                correct: 1 // b
            },
            {
                id: 30,
                source: "(EAGS 2018)",
                text: "Leia atentamente as afirmativas abaixo.<br>I – No vocábulo “alistar”, observa-se a derivação parassintética.<br>II – Os vocábulos “riacho”, “quietude” e “amanhecer” são formados por sufixos nominais.<br>III – “Automóvel” é formado por hibridismo.<br>Está(ão) correta(s) apenas a(s) afirmativa(s)",
                options: [
                    "a) I",
                    "b) II",
                    "c) I e III",
                    "d) II e III"
                ],
                correct: 2 // c
            },
            {
                id: 31,
                source: "(EAGS 2019)",
                text: "Assinale a alternativa em que todos os verbos são formados por derivação parassintética.",
                options: [
                    "a) desvalorizar, empalidecer, redistribuir.",
                    "b) desorientar, endurecer, esclarecer.",
                    "c) amanhecer, engordar, enfileirar.",
                    "d) rever, endireitar, desconsiderar."
                ],
                correct: 2 // c
            },
            {
                id: 32,
                source: "(EAGS 2020)",
                text: "Assinale a alternativa em que a palavra destacada forma-se por derivação imprópria.",
                options: [
                    "a) O tilintar das moedas em seu bolso incomodava-o. Um <b>judas</b>! Assim seria tratado.",
                    "b) Estava inquieto. Pensava nos afagos que receberia de sua namorada tão logo terminasse o dia.",
                    "c) Andava a pensar por onde diabo estaria o Zé, que não aparecera no botequim uma única <b>vezinha</b> na semana.",
                    "d) Enfim o perfume (e a ilusão)! Como trabalhara para poder ter em sua pele as <b>gotinhas</b> que lhe fariam ser notada e cortejada!"
                ],
                correct: 0 // a
            },
            {
                id: 33,
                source: "(EAGS 2021)",
                text: "Leia:<br>Ao contrário do vinho, o conhaque não <b>amadurece</b> na garrafa, apenas nos tonéis de carvalho. Para fazer-se um litro de <b>aguardente</b> que os ingleses chamam brandy, são necessários nove litros de vinho branco. Sua fermentação é feita <b>anualmente</b> em novembro, e a destilação dura os próximos três meses.<br>Assinale o processo de formação de palavras que não encontra exemplo no texto.",
                options: [
                    "a) Derivação parassintética ou parssintetismo",
                    "b) Composição por aglutinação",
                    "c) Derivação prefixal",
                    "d) Derivação sufixal"
                ],
                correct: 2 // c
            },
            {
                id: 34,
                source: "(EAGS 2022)",
                text: "Relacione as colunas quanto ao processo de formação das palavras. Em seguida, assinale a alternativa com a sequência correta.<br><br>1 – progresso<br>2 – monarquia<br>3 – resgate<br>4 – bem-aventurança<br><br>( ) derivação regressiva<br>( ) derivação prefixal<br>( ) composição por justaposição<br>( ) composição por aglutinação",
                options: [
                    "a) 3 - 1 - 4 - 2",
                    "b) 1 - 2 - 4 - 3",
                    "c) 3 - 1 - 2 - 4",
                    "d) 1 - 4 - 3 - 2"
                ],
                correct: 0 // a
            },
            {
                id: 35,
                source: "(EsPCEx 2011)",
                text: "Quanto à estrutura e formação de palavras, assinale a alternativa correta.",
                options: [
                    "a) Perfeição e percurso são palavras cognatas.",
                    "b) Em combatente, ocorre derivação parassintética.",
                    "c) A palavra pontiagudo é formada por justaposição.",
                    "d) Em exportar e êxodo, os prefixos têm sentido correspondente.",
                    "e) Em hipótese, o prefixo indica “antes, anterioridade”."
                ],
                correct: 3 // d
            },
            {
                id: 36,
                source: "(EsPCEx 2012)",
                text: "Assinale a alternativa em que todas as palavras são formadas por prefixos com significação semelhante.",
                options: [
                    "a) metamorfose – metáfora – meteoro – malcriado",
                    "b) apogeu – aversão – apóstata – abster",
                    "c) síncope – simpatia – sobreloja – sílaba",
                    "d) êxodo – embarcar – engarrafar – enterrar",
                    "e) débil – declive – desgraça – decapitar"
                ],
                correct: 1 // b
            },
            {
                id: 37,
                source: "(EsPCEx 2013)",
                text: "Assinale a alternativa que contém um grupo de palavras cujos prefixos possuem o mesmo significado.",
                options: [
                    "a) compartilhar - sincronizar",
                    "b) hemiciclo - endocarpo",
                    "c) infeliz – encéfalo",
                    "d) transparente - adjunto",
                    "e) benevolente - diáfano"
                ],
                correct: 0 // a
            },
            {
                id: 38,
                source: "(EsPCEx 2013)",
                text: "Ao se alistar, não imaginava que o <b>combate</b> pudesse se realizar em tão curto prazo, embora o <b>ribombar</b> dos canhões já se fizesse ouvir ao longe.<br>Quanto ao processo de formação das palavras sublinhadas, é correto afirmar que sejam, respectivamente, casos de",
                options: [
                    "a) prefixação, sufixação, prefixação, aglutinação e onomatopeia.",
                    "b) parassíntese, derivação regressiva, sufixação, aglutinação e onomatopeia.",
                    "c) parassíntese, prefixação, prefixação, sufixação e derivação imprópria.",
                    "d) derivação regressiva, derivação imprópria, sufixação, justaposição e onomatopeia.",
                    "e) parassíntese, aglutinação, derivação regressiva, justaposição e onomatopeia."
                ],
                correct: 1 // b
            },
            {
                id: 39,
                source: "(EsPCEx 2013)",
                text: "A alternativa que apresenta vocábulo onomatopeico é:",
                options: [
                    "a) Os ramos das árvores brandiam com o vento.",
                    "b) Hum! Este prato está saboroso.",
                    "c) A fera bramia diante dos caçadores.",
                    "d) Raios te partam! Voltando a si não achou que dizer.",
                    "e) Mas o tempo urgia, deslacei-lhe as mãos..."
                ],
                correct: 2 // c
            },
            {
                id: 40,
                source: "(EsPCEx 2013)",
                text: "São palavras primitivas:",
                options: [
                    "a) época – engarrafamento – peito – suor",
                    "b) sala – quadro – prato – brasileiro",
                    "c) quarto – chuvoso – dia – hora",
                    "d) casa – pedra – flor – feliz",
                    "e) temporada – narcotráfico – televisão – passatempo"
                ],
                correct: 3 // d
            },
            {
                id: 41,
                source: "(EsPCEx 2015)",
                text: "Responda, na sequência, os vocábulos cujos prefixos ou sufixos correspondem aos seguintes significados:<br>QUASE; ATRAVÉS; EM TORNO DE; FORA; SIMULTANEIDADE",
                options: [
                    "a) hemisfério; trasladar; justapor; epiderme; parasita",
                    "b) semicírculo; metamorfose; retrocesso; ultrapassar; circunavegação",
                    "c) penumbra; diálogo; periscópio; exogamia; sintaxe",
                    "d) visconde; ultrapassar; unifamiliar; programa; multinacional",
                    "e) pressupor; posteridade; companhia; abdicar; ambivalente"
                ],
                correct: 2 // c
            },
            {
                id: 42,
                source: "(EsPCEx 2018)",
                text: "Assinale a opção que identifica corretamente o processo de formação das palavras abaixo:",
                options: [
                    "a) qualidade – sufixação; saneamento – sufixação.",
                    "b) igualdade – sufixação; discriminação – parassíntese.",
                    "c) avanços – derivação imprópria; acesso – derivação regressiva.",
                    "d) acessível – prefixação; felizmente – sufixação.",
                    "e) planejamento – sufixação; combate – derivação regressiva."
                ],
                correct: 4 // e
            },
            {
                id: 43,
                source: "(EsPCEx 2019)",
                text: "Em “um item <b>insignificante</b> utilizado brevemente antes de ser descartado”, as palavras sublinhadas são formadas, respectivamente, por",
                options: [
                    "a) sufixação; derivação imprópria.",
                    "b) prefixação; derivação prefixal e sufixal.",
                    "c) aglutinação; hibridismo.",
                    "d) parassíntese; sufixação.",
                    "e) derivação prefixal e sufixal; parassíntese."
                ],
                correct: 3 // d
            },
            {
                id: 44,
                source: "(EsPCEx 2021)",
                text: "A palavra “obesogênico” é composta por radicais diferentes: “obeso”, de origem latina, e “gênico”, de origem grega, causando o que a gramática conceitua como hibridismo. É o que ocorre na palavra",
                options: [
                    "a) ultraprocessado.",
                    "b) antivírus.",
                    "c) televisão.",
                    "d) confinamento.",
                    "e) pandemia."
                ],
                correct: 2 // c
            },
            {
                id: 45,
                source: "(AFA 2012)",
                text: "Assinale a opção correta quanto à análise das palavras abaixo, em destaque, retiradas do texto II",
                options: [
                    "a) Os termos <b>indissociável</b> e <b>intransigente</b> são formadas somente pelo processo de derivação prefixal.",
                    "b) As palavras <b>ímpar</b> e <b>saída</b> seguem a regra de acentuação gráfica das vogais i e u tônicas dos hiatos.",
                    "c) Na frase, “...tinham...personalidades radicalmente <b>distintas</b>.” (l. 16 e 17), o termo distintas é sinônimo de notáveis.",
                    "d) Nas palavras destacadas em“...Gates ficou fascinado por Jobs e com uma <b>ligeira</b> inveja de seu efeito hipnótico...” (l. 37 e 38), há, respectivamente, dígrafo, dígrafo e encontro consonantal."
                ],
                correct: 3 // d
            },
            {
                id: 46,
                source: "(EFOMM 2011)",
                text: "O processo de formação de palavras que DESTOA dos demais aparece na palavra sublinhada na opção:",
                options: [
                    "a) (...) e depois a <b>injeção</b> que a enfermeira lhe passa.",
                    "b) (...) e desfaz com uma <b>espadeirada</b> todo o consultório (...).",
                    "c) O famoso pediatra, com um esgar colérico, recusa a <b>formidável</b> droga.",
                    "d) (...) para dar um beijo violento no seu amor de filho e também para preparar-lhe um <b>copázio</b> de vitaminas (...).",
                    "e) (...) dá um grito de horror e começa a chorar <b>nervosamente</b>."
                ],
                correct: 2 // c
            },
            {
                id: 47,
                source: "(EFOMM 2012)",
                text: "Assinale a opção cuja palavra sublinhada se forma por um processo diferente das demais.",
                options: [
                    "a) Eu indagava os rostos, pesquisava neles a furtiva <b>iluminação</b> (...).",
                    "b) (...) o traço de beatitude, que indicasse <b>conhecimento</b> (...).",
                    "c) Quem sabe se a riqueza, de que eu tinha medo, mas revestida de <b>doçura</b> (...).",
                    "d) (...) as pessoas se afastavam ou escondiam tão <b>finamente</b> tua posse (...).",
                    "e) Cheguei a supor que não existisses. Imaginei, às vezes (...)."
                ],
                correct: 4 // e
            },
            {
                id: 48,
                source: "(EFOMM 2013)",
                text: "Assinale a opção em que o processo de formação da palavra sublinhada é diferente das demais.",
                options: [
                    "a) Se não fosse ele, a <b>flagelação</b> me haveria causado menor estrago.",
                    "b) Sei que estava bastante zangado, e isto me trouxe a <b>covardia</b> habitual.",
                    "c) Débil e ignorante, incapaz de conversa ou defesa, fui encolher-me num <b>canto</b> (...).",
                    "d) Solto, fui enroscar-me perto dos caixões, coçar as <b>pisaduras</b> (...).",
                    "e) As minhas primeiras relações com a justiça foram <b>dolorosas</b> (...)."
                ],
                correct: 2 // c
            },
            {
                id: 49,
                source: "(EFOMM 2015)",
                text: "Assinale a opção em que o processo de formação da palavra sublinhada é diferente dos demais.",
                options: [
                    "a) Mas possuía o que qualquer criança <b>devoradora</b> de histórias gostaria de ter: um pai dono de livraria.",
                    "b) Até que veio para ela o <b>magno</b> dia de começar a exercer sobre mim uma tortura chinesa.",
                    "c) Mas que talento tinha para a <b>crueldade</b>.",
                    "d) Comigo exerceu com calma <b>ferocidade</b> o seu sadismo.",
                    "e) Como casualmente, informou-me que possuía ‘As <b>reinações</b> de Narizinho’, de Monteiro Lobato."
                ],
                correct: 1 // b
            },
            {
                id: 50,
                source: "(EFOMM 2016)",
                text: "No que tange ao processo de formação de palavras, o termo destacado que se enquadra como formação regressiva aparece na opção",
                options: [
                    "a) As comidas, para mim, são entidades oníricas. Provocam a minha capacidade de <b>sonhar</b>.",
                    "b) Um bom pensamento nasce como uma pipoca que estoura, de forma inesperada e <b>imprevisível</b>.",
                    "c) É que a transformação do milho duro em pipoca macia é símbolo da grande <b>transformação</b> (...)",
                    "d) O <b>estouro</b> das pipocas se transformou, então, de uma simples operação culinária (...)",
                    "e) O milho da pipoca somos nós: duros, quebra-dentes, impróprios para <b>comer</b> (...)"
                ],
                correct: 3 // d
            },
            {
                id: 51,
                source: "(EFOMM 2017)",
                text: "Quanto ao processo de formação de palavras, o de conversão NÃO está presente na palavra sublinhada na alternativa",
                options: [
                    "a) Disse certo o poeta: ‘<b>Navegar</b> é preciso ’, a ciência da navegação é saber preciso (...)",
                    "b) O ritmo das remadas acelera. Sabem tudo sobre a ciência do <b>remar</b>.",
                    "c) (...) multiplicam-se os meios técnicos e científicos ao nosso dispor, que fazem com que as <b>mudanças</b> sejam cada vez mais rápidas (...)",
                    "d) Em relação à vida da sociedade, ela contém a <b>busca</b> de uma utopia.",
                    "e) A nau navega veloz e sem rumo. Nas universidades, essa <b>doença</b> (...)"
                ],
                correct: 3 // d
            },
            {
                id: 52,
                source: "(EFOMM 2018)",
                text: "Assinale a opção na qual a palavra sublinhada se formou por um processo diferente das demais.",
                options: [
                    "a) Estaremos debaixo da <b>goiabeira</b>; eu cortarei uma forquilha com o canivete.",
                    "b) Mas eu a levarei para a beira do ribeirão, na sombra fria do <b>bambual</b> (...).",
                    "c) Tinha uma que era para mim uma <b>adoração</b>.",
                    "d) Ardente da mais pura paixão de beleza é a <b>adoração</b> da infância.",
                    "e) Preciso de um <b>sossego</b> de beira de rio, com remanso, com cigarras."
                ],
                correct: 4 // e
            },
            {
                id: 53,
                source: "(Escola Naval 2014)",
                text: "Em que opção o comentário sobre as palavras sublinhadas está correto?",
                options: [
                    "a) \"E o irmão lá atrás, <b>respeitoso</b>, [...].\" (3°§) - o sufixo \"-oso\" forma o adjetivo sublinhado a partir de um substantivo.",
                    "b) \"Não, não enchi a <b>garrafinha</b> de água salgada para [...].\" (4°§) - em \"garrafinha\", o sufixo de diminutivo tem valor pejorativo.",
                    "c) \"[...] o carioca, com esse modo natural de ir à praia, <b>desvaloriza</b> o mar.\" (6°§)- o prefixo \"des-\" denota repetição em \"desvalorizar\".",
                    "d) \"Ele vai ao mar com a <b>sem-cerimônia</b> que o mineiro vai ao quintal.\" (6°§) - \"sem-cerimônia\" é um neologismo formado por aglutinação.",
                    "e) \"[...] que surpresas <b>ondearão</b> entre a lareira e a mesa de vinhos e queijos!\" (7°§) - o verbo sublinhado é formado por prefixação."
                ],
                correct: 0 // a
            },
            {
                id: 54,
                source: "(Escola Naval 2014)",
                text: "Em que opção houve mudança de classe gramatical do termo destacado?",
                options: [
                    "a) \"Era um <b>oásis</b> a caminhar.\" (2°§)",
                    "b) \"E o irmão lá atrás, <b>respeitoso</b>, era a sentinela, [...].\" (3°§)",
                    "c) \"[...] convertendo a <b>arma</b> em caneta ou lápis [...].\" (3°§)",
                    "d) \"[...] que na mesa interior marulhavam <b>lembranças</b> [...].\" (4°§)",
                    "e) \"O mar é um <b>morrer</b> sucessivo e [...].\"(8°§)"
                ],
                correct: 4 // e
            },
            {
                id: 55,
                source: "(Escola Naval 2016)",
                text: "Em que opção encontra-se uma palavra, cujo processo de formação é o mesmo do termo destacado em \"[...] o tão sonhado <b>embarque</b> [...].\" (11°§)",
                options: [
                    "a) \"[...] <b>circundam</b> minha terra[...].\" (2°§)",
                    "b) \"[...] não seriam certamente em <b>vão</b>.\" (4 °§)",
                    "c) \"E o sentimento de <b>perda</b> [...].\" (6a§)",
                    "d) \"Seis anos <b>entremeados</b> de aulas, [...].\" (7°§)",
                    "e) \"[...] o notável <b>escritor-marinheiro</b> [...].\" (13°§)"
                ],
                correct: 2 // c
            },
            {
                id: 56,
                source: "(Escola Naval 2017)",
                text: "Leia o fragmento:<br>“[...] a relação entre o <b>Eu</b> e o Universo [...]’’ (2°§)<br>Assinale a opção em que a palavra sublinhada foi formada pelo mesmo processo que a palavra destacada acima.",
                options: [
                    "a) O beija-flor voou feliz até o <b>ninho</b>.",
                    "b) <b>Infeliz</b> aquele que não ama o próximo.",
                    "c) - <b>Silêncio</b>! Pediu o professor à turma.",
                    "d) O <b>resgate</b> dos feridos aconteceu normalmente.",
                    "e) A falta cometida pelo jogador foi <b>desleal</b>."
                ],
                correct: 2 // c
            },
            {
                id: 57,
                source: "(Escola Naval 2018)",
                text: "Assinale a opção em que o processo de formação da palavra “inalienável” (3°§) está corretamente indicado.",
                options: [
                    "a) Derivação imprópria.",
                    "b) Formação regressiva.",
                    "c) Derivação prefixal e sufixal.",
                    "d) Composição por justaposição.",
                    "e) Derivação parassintética."
                ],
                correct: 2 // c
            },
            {
                id: 58,
                source: "(Escola Naval 2019)",
                text: "Assinale a opção cujo termo destacado exemplifica o mesmo processo de formação de palavras observado em: “Há também os grandes <b>clássicos</b>, os romances que todos amamos e queremos ter ao alcance da mão.” (6°§)",
                options: [
                    "a) <b>Ler</b>, sem dúvida, é uma atividade prazerosa e de esforço intelectual.",
                    "b) Os estudantes apreciaram o vídeo com entrevistas de antigos <b>escritores</b>.",
                    "c) Seria lamentável, realmente, a morte dos livros <b>impressos</b> em papel.",
                    "d) Um livro escolar de boa qualidade consegue <b>esclarecer</b> as dúvidas dos estudantes.",
                    "e) Acreditamos que a leitura de um bom livro seja um dos melhores <b>passatempos</b>."
                ],
                correct: 0 // a
            },
            {
                id: 59,
                source: "(Escola Naval 2020)",
                text: "Assinale a opção em que o comentário sobre o termo sublinhado está correto.",
                options: [
                    "a) \"O Menino vislumbrava. Respirava muito.\" (5°§) - o termo em destaque é formado por um processo de composição por justaposição.",
                    "b) \"O Menino riu, com todo o coração. Mas só <b>bis-viu</b>. Já o chamavam, para passeio.\" (6°§) - o termo em destaque é um neologismo.",
                    "c) \"Talvez não devesse, não fosse direito ter por causa dele aquele <b>doer</b>, que põe e punge, de dó, desgosto e desengano.\" (10 o§) - o termo em destaque é formado por derivação imprópria.",
                    "d) \"[...] e que entre o contentamento e a desilusão, na balança infidelíssima quase nada <b>medeia</b>.\" (10°§) - o termo em destaque encontra-se no grau superlativo absoluto analítico.",
                    "e) \"Seu pensamentozinho estava ainda na fase hieroglífica.\" (11o§) - o termo em destaque encontra-se no diminutivo e tem valor pejorativo."
                ],
                correct: 1 // b
            }
        ];

        // State Management
        let score = 0;
        let answered = 0;
        let userAnswers = {}; // Format: { questionIndex: selectedOptionIndex }

        // DOM Elements
        const container = document.getElementById('quiz-container');
        const scoreDisplayEl = document.getElementById('score-display');
        const wrongDisplayEl = document.getElementById('wrong-display');
        const totalQuestionsDisplayEl = document.getElementById('total-questions-display');
        const progressBarCorrect = document.getElementById('progress-bar-correct');
        const progressBarWrong = document.getElementById('progress-bar-wrong');
        const STORAGE_KEY = 'quiz_portugues_morfologia_progress';

        // Initialize
        function initQuiz() {
            container.innerHTML = '';
            quizData.forEach((q, index) => {
                const questionHTML = `
                    <div class="bg-white rounded-lg p-6 mb-6 question-card border-l-4 border-indigo-500" id="card-${index}">
                        <div class="flex justify-between items-start mb-4">
                            <span class="text-xs font-bold text-indigo-600 uppercase tracking-wide bg-indigo-100 px-2 py-1 rounded">${q.source}</span>
                            <span class="text-gray-400 text-sm font-mono">Questão ${q.id}</span>
                        </div>
                        ${q.context ? `<div class="mb-4 text-sm text-gray-700 bg-gray-100 p-3 rounded border border-gray-200">${q.context}</div>` : ''}
                        <h3 class="text-lg font-medium text-gray-800 mb-6 leading-relaxed">${q.text}</h3>
                        <div class="space-y-3">
                            ${q.options.map((opt, i) => `
                                <div class="relative">
                                    <input type="radio" name="question-${index}" id="q${index}-opt${i}" value="${i}" class="option-input hidden peer" onchange="checkAnswer(${index}, ${i})">
                                    <label for="q${index}-opt${i}" id="label-${index}-${i}" class="option-label block w-full p-4 text-sm md:text-base text-gray-700 bg-white border border-gray-300 rounded-lg cursor-pointer hover:bg-gray-50 peer-checked:border-indigo-600 peer-checked:text-indigo-600 shadow-sm">
                                        ${opt}
                                    </label>
                                    <div id="feedback-${index}-${i}" class="absolute right-4 top-4 hidden"></div>
                                </div>
                            `).join('')}
                        </div>
                        <div id="result-${index}" class="mt-4 hidden p-3 rounded-lg text-sm font-semibold"></div>
                    </div>
                `;
                container.innerHTML += questionHTML;
            });
            score = 0;
            answered = 0;
            userAnswers = {};
            
            // Try to load saved progress
            loadSavedProgress();
            
            updateScoreUI();
        }

        function checkAnswer(questionIndex, selectedOptionIndex) {
            // Prevent changing answer once selected
            if (userAnswers[questionIndex] !== undefined) return;

            const question = quizData[questionIndex];
            
            // Store the selected option index, not just boolean
            userAnswers[questionIndex] = selectedOptionIndex;
            
            // Save immediately
            saveProgress();

            // Update Logic
            const isCorrect = selectedOptionIndex === question.correct;
            answered++;
            if (isCorrect) score++;

            // Visual feedback
            updateQuestionUI(questionIndex, selectedOptionIndex, isCorrect, question.correct);
            updateScoreUI();
        }
        
        function updateQuestionUI(questionIndex, selectedOptionIndex, isCorrect, correctOptionIndex) {
             const selectedLabel = document.getElementById(`label-${questionIndex}-${selectedOptionIndex}`);
            const correctLabel = document.getElementById(`label-${questionIndex}-${correctOptionIndex}`);
            const resultDiv = document.getElementById(`result-${questionIndex}`);
            
             // Disable all inputs for this question
            const inputs = document.getElementsByName(`question-${questionIndex}`);
            inputs.forEach(input => input.disabled = true);
            
            // Mark the selected input as checked (important for reload)
            const selectedInput = document.getElementById(`q${questionIndex}-opt${selectedOptionIndex}`);
            if(selectedInput) selectedInput.checked = true;

            if (isCorrect) {
                if(selectedLabel) {
                    selectedLabel.classList.add('correct-answer');
                    if(!selectedLabel.innerHTML.includes('fa-check')) {
                        selectedLabel.innerHTML += ' <i class="fas fa-check ml-2"></i>';
                    }
                }
                resultDiv.className = "mt-4 block p-3 rounded-lg text-sm font-bold bg-green-100 text-green-800 border border-green-200";
                resultDiv.innerHTML = '<i class="fas fa-check-circle mr-2"></i>Resposta Correta!';
            } else {
                if(selectedLabel) {
                    selectedLabel.classList.add('wrong-answer');
                    if(!selectedLabel.innerHTML.includes('fa-times')) {
                         selectedLabel.innerHTML += ' <i class="fas fa-times ml-2"></i>';
                    }
                }
                if(correctLabel) {
                    correctLabel.classList.add('correct-answer'); // Show correct answer
                    if(!correctLabel.innerHTML.includes('fa-check')) {
                        correctLabel.innerHTML += ' <i class="fas fa-check ml-2"></i>';
                    }
                }
                
                resultDiv.className = "mt-4 block p-3 rounded-lg text-sm font-bold bg-red-100 text-red-800 border border-red-200";
                resultDiv.innerHTML = '<i class="fas fa-exclamation-circle mr-2"></i>Resposta Incorreta. A correta está destacada em verde.';
            }
        }

        function updateScoreUI() {
            const wrong = answered - score;
            const total = quizData.length;
            
            scoreDisplayEl.innerText = score;
            wrongDisplayEl.innerText = wrong;
            totalQuestionsDisplayEl.innerText = total;
            
            const percentageCorrect = total > 0 ? (score / total) * 100 : 0;
            const percentageWrong = total > 0 ? (wrong / total) * 100 : 0;
            
            progressBarCorrect.style.width = `${percentageCorrect}%`;
            progressBarWrong.style.width = `${percentageWrong}%`;
        }

        function saveProgress() {
            localStorage.setItem(STORAGE_KEY, JSON.stringify(userAnswers));
        }

        function loadSavedProgress() {
            const saved = localStorage.getItem(STORAGE_KEY);
            if (saved) {
                try {
                    userAnswers = JSON.parse(saved);
                    // Iterate through saved answers and restore UI
                    for (const [qIndex, optIndex] of Object.entries(userAnswers)) {
                        const qIdx = parseInt(qIndex);
                        const oIdx = parseInt(optIndex);
                        
                        // Validate indices exist
                        if(quizData[qIdx] && quizData[qIdx].options[oIdx]) {
                            const isCorrect = oIdx === quizData[qIdx].correct;
                            answered++;
                            if(isCorrect) score++;
                            
                            updateQuestionUI(qIdx, oIdx, isCorrect, quizData[qIdx].correct);
                        }
                    }
                } catch (e) {
                    console.error("Error loading progress:", e);
                    localStorage.removeItem(STORAGE_KEY);
                }
            }
        }

        function resetQuiz() {
            if(confirm("Deseja reiniciar o quiz? Todo o seu progresso será perdido.")) {
                localStorage.removeItem(STORAGE_KEY);
                window.scrollTo(0,0);
                initQuiz();
            }
        }

        // Start
        initQuiz();

    </script>
</body>
</html>
