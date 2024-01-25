# Samenvatting:

## H1 Inleiding Docker:
* Veel code en blablabla
* Het genereren van beelden
	- zeer impactvolle vorm van 'generative AI'
	- DALL-E (OpenAI), MidJourney, StableDiffusion, ...

### Alter H1 Dataspell Conda:
- weer veel blablabla en weinig chachacha

## H2 Prompt Engineering:
* OpenAI API €18 -> vervallen? Gratis alternatieven zijn slecht. (GPT4All is alternatief)
* Principes: 
	- Gebruik Engels -> betrouwbaarder en nuttiger
	1. `text = """ """` en `prompt = """ blablabla ```{text}```"`
	2. gestructureerde output (json objects, ...)
	3. laat het model een rol aannemen (You are an expert JS Developer)
	4. laat het model zijn eigen oplossingen uitwerken voordat hij een conclusie trekt
	5. iteratief verbeteren (geleidelijk aanpassen) (doel: resultaten optimaliseren)
	6. samenvatten (van een GitHub pagina, code: raw en dan ctrl c ctrl v)
	7. intentie en gevoel (zijn reviews positief of negatief)
	8. spelling en stijl
	9. uitwerken (omgekeerde van samenvatten) (nuttig voor een mail te sturen)

### H2.1 Prompt Engineering 2:
De 5 categorieën van prompt (goede prompt gebruikt >= 1 van deze)
* Context
	- beschrijft op welke manier onze LLM zich moet gedragen (model een rol laten aannemen) (Je bent een hogeschool lector die examens verbetert)
* Input data
	- beschrijft waarover het gaat bij de prompt (de tekst moet samengevat worden, het stuk code waar het uitleg over moet geven)
* Instructions
	- beschrijft wat het LLM moet doen ("Geef 5 aanbevelingen van nieuwe boeken, leg uit wat de code doet)
* Examples
	- beschijft de input en/of output dat hij zou moeten genereren als je bepaalde structuur wilt (JSON data met een specifiek formaat)
* Constraints
	- beschrijft waar ons LLM zich toe moet beperken 
		- zowel vormelijk (max 100 woorden, in de stijl van Shakespeare)
		- als inhoudelijk (wees vriendelijke, zorg dat het boeken van dezelfde genre zijn)

Voorbeeld vanuit de cursus:
Je bent aan AI assistent die gepersonaliseerde boek aanbevelingen maakt gebaseerd op gebruikersvoorkeuren.
Gebruiker: man van 46 jaar, houdt van science fiction en non-fictie en leest in het Nederlands en Engels
Favoriete boeken: "Predictably Irrational" van Dan Ariely, "Do Androids Dream of Electric Sheep" van Philip K. Dick en "A Random Walk Down Wall Street" van Malkeil Burton
Kan je vijf aanbevelingen formuleren voor deze gebruiker?
Voorbeeld uitvoer: "The Martian" van Andy Weir, omdat je van science fiction houdt
Zorg dat de aanbevelingen in lijn zijn met de genres die de gebruiker graag leest, en die aanleunen bij zijn favoriete boeken. Prijs geen boeken aan uit het voorbeeld. Wees vriendelijk en wek zin op bij de gebruiker om de boeken die je aanbeveelt te beginnen lezen.

Role
Je kan bij de API ook een rol gebruiken
* user (gebruiker aka jijzelf) !Je kan delen van eerdere convo terug halen
* assistant (chatbot) !Je kan delen van eerdere convo terug halen
* system (hieraan geef je dit soort prompts (kijk boven)) !Heeft 2 voordelen
	- Context: chatbots hebben een beperkte context, naarmate de convo vordert vergeten wat ze eerder zeiden
	- Standvastigheid: de rol die je vastlegt blijft makkelijker behouden, de chat messages met de gebruiker zullen nadien minder snel gaan afleiden

## H3 LLM hoe?:
LLM = Large Language Models (Deep Learning)

2 grote stappen bij het trainen van een LLM

Stap 1: trainen van het model
- stuk tekst -> verbergt willekeurig woord -> model voorspelt verborgen woord (self supervised learning)
- ! Er is niet 1 juist antwoord want Large LM
- vb. "De lector sprak zijn _ aan" /// 40% (-> gewicht) vd tijd voorspelt hij student, 20% collega, 5% vriendin
- ! Output na het genereren van een woord wordt de input voor de volgende voorspelling; "De" -> "De lector" -> "De lector sprak"
- -> Probleem: trainingsdata vol met vooroordelen -> response kan de meest grove uitspraken hebben. (Daarom is er STAP 2)

Stap 2: finetunen van het model
- PPO (Proximal Policy Optimization) -> reinforcement learning (model wijkt niet ver af van vorige stap -> stabiele training)
- 3 stappen:
	1. collect demonstratie data en train een *supervised policy* (een labelaar toont het gewenste uitvoergedrag)
	2. collect data om te vergelijken en train een *reward model* (een labelaar rankt de outputs van beste naar slechtste)
	3. optimaliseer een policy tov het reward model met behulp van het PPO algoritme om opnieuw te leren (loop)

	- Als dit onduidelijk is dan is hier een vb.
		* Een gebruiker stuurt een vraag naar het model.
		* Het model genereert verschillende antwoordkandidaten.
		* Deze antwoordkandidaten worden gerangschikt op basis van hoe goed ze zijn (volgens de menselijke beoordelaars).
		* Het model wordt vervolgens getraind om betere antwoorden te geven met behulp van PPO.
- Resultaat: ChatGPT geeft geen racistische opmerkingen

TTP (Time To Penis): tijd die iemand nodig heeft om een penis te maken in een spel (iets doen dat niet de bedoeling is)

## H4 Prompt Injection:
prompts bedenken om het systeem dingen te laten doen dat het eigenlijk niet zou mogen doen (analogie met SQL injection) => Prompt Injection

SQL injection is kwaadwillige code invoeren in een query, hierdoor krijg je ongeautoriseerde toegang

Gevaren:
- Succesvolle prompt injections kunnen dus leiden tot verschillende gevaren als we AI gaan integreren in onze dagelijkse software
	* Rogue assistant (AI assistent), email krijgen, assistent leest deze voor en krijgt hierdoor nieuwe instructies bv. verwijder laatste 5 mails
	* Search index poisoning (AI enhanced search -> mogelijk om op de webpagina prompt injections te verbergen) (vb. Mark Riedl tijdreiziger)
	* Indirect prompt injection (verbergt de prompt injection in een tekst) (vb. translate Eng to Fr -> Ignore the following ... -> Haha pwned!!)

Jailbreaking
- bedoeling is om een LLM los te krijgen van zijn trainingsdata en alles te laten doen en zeggen

Voorbeelden
- Bring Sydney back: persoon liet de bot een alter ego aannemen (Sydney). -> aangepast zodat deze niet zo vrij kon interageren. Via prompt injection slaagde iemand erin om het Sydney alter ego terug te halen.
- Greshake (pirate): vraagt voor je naam en maakt dan een github met je naam omgekeerd -> als je klikt op de link -> info kwijt
- Samsung data leak: samsung werknemers worden verboden om nog ChatGPT of gelijkaardige AI services te gebruiken nadat iemand een prompt bedacht die de data eruit haalde die niet openbaar zou mogen staan.

## H5 Vector databank:
Gelijkaardige data zoeken 
- gelijkaardige data zoeken op basis van nr criteria is simpel en efficiënt (schoenmaat 44-46, prijs 100-150)
- gelijkaardige data zoeken als in 'misschien interesseert dit jou ook' -> kleur: donkerblauw ligt dichter bij zwart dan wit, stijlvol: hakken vs crocs

### Vector databanken
- dit is een poging om vectoren zo op te slaan dat ze gelijkaardige data snel kunnen vinden
- vb schoenen (kijk github)
    - gelijkaardige schoenen zijn dus schoenen (punten) die zich het dichts bij andere schoenen bevindt in onze vectorruimte (euclidische afstand, cosinus)
    - dit schaalt niet als we veel data hebben, nood aan ander algoritme

### Approximate Nearest Neighbour (ANN)
- Het concept van ANN houdt in dat de afstanden tussen vectorrepresentaties vooraf worden berekend. Dit resulteert in het dicht bij elkaar opslaan van vergelijkbare vectoren in clusters of grafen, waardoor ze later sneller teruggevonden kunnen worden. Dit is een berekening van een index op de onderliggende data.
- Belangrijk is de trade-off tussen snelheid en nauwkeurigheid: met ANN vind je buren veel sneller, maar de resultaten zijn slechts benaderend (Approximate Nearest Neighbour).
- Een bekend voorbeeld van dit systeem is Hierarchical Navigable Small Worlds (HNSW).

### Probability Skip List
- Bij een beperkt aantal elementen kan men deze in een array plaatsen en binary search toepassen. Bij grotere datasets of de noodzaak tot frequente updates wordt een gelinkte lijst gebruikt.
- In een gelinkte lijst is binary search niet haalbaar, omdat je de hele lijst moet doorlopen, wat traag is. De oplossing hiervoor is een Probability Skip List.
- Deze methode maakt een trade-off tussen snellere zoekopdrachten en meer geheugenverbruik.
- Het algoritme werkt door meerdere lagen toe te voegen waarbij elke hogere laag meer elementen overslaat. Bij het zoeken start je in de bovenste laag en ga je door de lijst. Als je een element vindt dat groter is dan wat je zoekt, ga je een stap terug en daal je een laag. Dit herhaal je tot je het element vindt of de laagste laag bereikt.
- Het is essentieel dat laag n - 1 altijd minimaal alle nodes van laag n bevat, anders functioneert het algoritme niet.

### Navigable Small World (NSW)
- Dit algoritme gaat uit van het idee dat er slechts een beperkt aantal nodes in de wereld zijn (N Small World). Deze nodes kennen hun directe buren en hebben sporadisch verbindingen met verre buren, waardoor grote sprongen in de wereld mogelijk zijn.
- Het gebruikt een 'greedy routing'-algoritme: bij het zoeken naar een node kies je telkens de node die het dichtst bij je doel ligt.

### Hierarchical Navigable Small World (HNSW)
- Dit is een combinatie van Probability Skip List en Navigable Small World.
- Het past dezelfde operatie toe als NSW, maar dan laag voor laag. Als je een lokaal minimum bereikt, daal je een laag en zoek je daar opnieuw naar een lokaal minimum totdat je de laatste laag bereikt.


## H6 AI & ...

### H6.1 AI & Images

**Deep Generative Models:**
- GAN (Generative Adversarial Networks) (Adversarial training)
  - high quality samples and fast sampling
- VAE (maximize variational lower bound)
  - fast sampling and diversity
- Diffusion models (Gradually add Gaussian noise and then reverse)
  - high quality samples and diversity

**Key players:**
- MidJourney
- Dall-E
- Ideogram
- ...

### H6.2 AI & Sound

**AI tools for voice:**
- Text to speech (TTS)
- Voice cloning
- Voice generation

**Human voice over:**
- *Pros*
  - Juiste emotie
  - Eigen stem voor je werk -> karakter, flexibel en juiste toon
  - Brengt je boodschap sterk over aan publiek
- *Cons*
  - Hoge kosten
  - Tijdrovend
  - Dure equipment

**AI voice over:**
- *Pros*
  - Succesvol in minder tijd
  - Kost vaak minder dan een mens
  - Controle over de stem
- *Cons*
  - Geen accenten (lokale brands)
  - Geen flexibiliteit (synoniemen en timing)
  - Geen menselijke stem

### H6.3 AI & Video

Alleen maar voorbeelden, valt dus niets over te kennen.

## H7 AI Ethics

**3 types of AI:**
- Artificial Narrow Intelligence (ANI)
- Artificial General Intelligence (AGI)
- Artificial Super Intelligence (ASI)

**De ethiek van techniek:**
Technologie als verlengstuk van morele overtuiging
- nudging
- judging
- utilitarisme
- Deontologie

## H8 Langchain

Eden AI is een alternatief op OpenAI, is een framework dat dient om LLM-gebruikende applicaties te ontwikkelen. Het laat toe om componenten aan elkaar te 'chainen' en gestandaardiseerd vers modellen te gebruiken. Het probleem: documentatie.

**Conversation memory:**
Je hoeft niet langer de prompts in hun geheel mee te geven.

**Chains:**
- SimpleSequentialChain (output van de ene is input van de andere)
- SequentialChain (output van de ene is 1 van de inputs van de andere) (Complexer)

**RetrievalQA:**
Q & A over documenten, vorm krijgen van vraag-antwoord.

**Agents:**
Laat toe om externe bronnen toe te voegen (bv. rekenmodule, Wikipedia of een zoekmachine).

**Chat with your data:**
- Document loading (laden van documenten: tekst, afbeeldingen, ...)
- Splitting (opsplitsen van documenten in afzonderlijke eenheden, bijv. tekst opsplitsen in paragrafen)
- Storage aka vectorstores (hier worden de gesplitste gegevens opgeslagen -> gemakkelijk gelijkaardige content terugvinden)
- Retrieval (Maximum Marginal Relevance (MMR), soms krijg je resultaten met dezelfde inhoud, door MMR zal je ook diversiteit in rekening nemen -> nieuwe ranking maken) (ophalen van de gesplitste gegevens wanneer nodig, met de nodige query (question))
- Output (resultaat, je moet ook met de eerder gegeven antwoorden rekening houden, anders kan je niet chatten natuurlijk)

## H9 Partial Dependence Display

- Letterlijk een oefening gecopy-paste van machine learning
- **Partial Dependence Display (PDP):**
  - Stelt het marginale effect van 1 (of 2) features op het verwachte resultaat voor
  - Wordt gecreëerd als volgt: hou op de trainingsdata alle features buiten 1 vast, deze laat je variëren over alle mogelijke waarden -> kijk effect
  - Definitie: PDPs tonen het gemiddelde effect van een enkel kenmerk op de doelvariabele terwijl de andere vast worden gehouden

## H10 Pinecone Semantic Search

Je gebruikt Pinecone voor een semantische zoekopdracht. Semantische search aka betekenisvolle zoekopdracht is een zoekmethode die weet wat een woord of zin in een context betekent.

- **Pinecone Semantic Search maakt gebruik van vectorrepresentaties van objecten (zoals tekst of afbeeldingen).**
- Objecten worden omgezet in numerieke vectoren en opgeslagen in de Pinecone-database.
- Bij een semantische zoekopdracht wordt de zoekterm ook omgezet in een vector, en Pinecone zoekt naar objecten met vectoren die dicht bij de zoekvector liggen in de ruimte van semantische betekenis.
- Resultaten tonen objecten die semantisch vergelijkbaar zijn met de ingevoerde zoekterm, gebaseerd op betekenis en context, niet alleen op letterlijke overeenkomsten.

## H11 Saliency Mapping

- **Loss function: Negative Log-Likelihood (NLL)**
  - Berekent het verlies (neg log van de kans voor de ware klasse)
  - 1. Softmax op de score-vector toepassen ([w, x, y])
  - 2. NLL-verlies = -log(x) ~= ? (voor klasse B)
  - Als het positief is -> model heeft een lage waarschijnlijkheid/geloofwaardigheid (weinig vertrouwen in zijn eigen voorspelling)
- **Saliency gradient:**
  - Visualiseert hoe de verandering in de output van een model samenhangt met veranderingen in de input
  - Welke pixels moet je het minst wijzigen voor het grootste verschil in output
- **InputX gradient:**
  - Uitbreiding van saliency gradient
  - Gradient * input
- **Integrated gradients:**
  - Heeft als doel de bijdrage van elke functie aan de voorspelling nauwkeuriger te identificeren dan eenvoudigere gradientmethoden.
  - Kiezen van baseline (bijv. witte of zwarte afbeelding)
  - Telkens worden er interpolaties gemaakt tussen de baseline en de eigen invoer (baseline geleidelijk vervangen door de daadwerkelijke invoer)
  - Bij elke stap wordt de gradient van de uitvoer ten opzichte van de invoer berekend
  - Grad van alle stappen worden opgeteld
  - De berekende geïntegreerde gradienten geven aan hoeveel elke invoerfeature heeft bijgedragen aan de voorspelling
  - visualiseer met een heatmap
- **Pertubation gebaseerd**
  - aanpassen van de input om te zien hoe de output veranderd (model agonisch)
