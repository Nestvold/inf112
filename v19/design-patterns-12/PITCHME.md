---

### Designmønstre

Note: 
- Vanlige, etablerte løsninger på velkjente problemer innen programvareutvikling
- Typisk en klasseoppdeling med navnekonvensjon i realiteten
- Konkrete fremgangsmåter for å følge SOLID-prinsippene

---

### Creational patterns

Note:
- designmønstre som handler om hvordan vi oppretter objekter

---

### Singleton

Note:
- hva er singleton?
- En måte å bare opprette et objekt på
- hvilket problem løser singleton?
- når vi ikke kan ha mange instanser av en klasse liggende
- feks: databasekoblinger, vi vil ikke ha en ny kobling til databasen hver gang
  en request kommer inn, da slutter alt å virke
- mer generelt: hvis en instans av et objekt kan knyttes direkte til ressursbruk
  (minne, CPU, koblinger over nett osv), kan dette være et tegn på at mengden
  instanser skal begrenses
- hvis en instans av en klasse holder på tilstand i et system (flere instanser
  kan føre til race conditions eller ugyldige tilstander i programmet)
- alle i systemet må få tilgang til instansen
- kan styre når instansen skal tilordnes og initialiseres
- hvordan implementerer vi en singleton?
- klasse (<navn>Singleton), har private static variabel for instans som skal
  være singleton
- klassen har privat konstruktør (dette gjør at ingen kan kalle den, og den kan
  heller ikke subklasses)
- public static-metode for å hente ut instansen getInstance() som oppretter
  instans når den trengs og ellers returnerer instansen
- singleton brukes gjerne av mer avanserte designmønstre
- alternativ til implementasjon: lag en enum med bare en type (som er instansen
  du skal opprette)

---

### Factory

Note:
- en måte å lage instanser av subklasser på uten å eksponere dem til toppklasser
- Hvilket problem løser Factory?
- lar høynivåklasser få tilgang til subklasser uten å måtte avhenge av de
  spesifikke subklassene
- dependency inversion: klasser skal avhenge av abstrakte klasser, ikke
  spesifikke implementasjoner (klasser skal avhenge av konsepter heller enn
  konkrete eksempler)
- finnes flere varianter av denne, vi går gjennom klassisk Factory
- Hvordan implementerer vi Factory?
- lager et interface som heter <navn>Factory
- lag en metode for hver konkrete klasse som finnes
- disse metodene implementeres i en konkret Factory-klasse. Hver metode
  returnerer et objekt av den konkrete klassen som ønskes
- metodene returnerer supertypen (feks Shape), slik at toppklassen som kaller
  metoden ikke trenger å forholde seg til hvilken konkret implementasjon som
  brukes
- eksempel på dette: Collection i java
- Relatert til dette: Factory Method, som også har med å instansiere objekter av
  konkrete klasser å gjøre. Men: implementert litt annerledes, hvor du avgjør
  hvilken implementasjon som skal returneres der og da (feks basert på filnavn
  eller andre attributter). I dette tilfellet har du bare en metode, men den må
  utvides/endres hver gang en ny klasse kommer til eller faller fra


---

### Builder

Note:
- hva er builder?
- en måte å lage objekter på som frakobler parametre fra konstruktør
- hvilket problem løser Builder?
- opprettelse av objekt der du trenger mange konstruktører for å kunne ta høyde
  for alle varianter av kombinasjoner av parametre
- opprette objekter der det er mange optional-parametre
- brukes typisk der det er mange parametre
- må lage egen klasse, så kontekstuelt litt mer avansert å forholde seg til, men
  kan gi større klarhet i kode fordi det er lett å lage objekter med riktige
  parametre som ikke gjemmes vekk for andre som leser koden (hvis man feks må
  oppgi 10 parametre mens det egentlig bare er fem som er viktige for den
  instansen)
- brukes ofte i forbindelse med testing
- Hvordan gjøres dette?
- Du lager en privat konstruktør i klassen din
- Du lager en Builder-klasse inne i klassen din
- Du kaller konstruktøren til Builder-klassen, og den returnerer et
  Builder-objekt 
- Du kan så kalle metoder på builder-objektet for å sette nødvendige parametre
- Til slutt kaller du build-metoden på Builder-objektet, og denne returnerer
  objektet du egentlig vil lage (siden Builder-klassen er inne i klassen du
  opererer på, får den lov å kalle den private konstruktøren i klassen)
- Litt mer å forstå, men mye mer fleksibel bruk
- gir mulighet for objekter som ikke kan endres, immutable objects


---

### Behavioural patterns

Note: 
- handler om å gjøre det enklere å velge riktig type oppførsel ved å innkapsle
  oppførsel i klassestrukturer


---

### Strategy

Note: 
- eksempel: 
- Fagsystem for produksjon av vinduer (lage tekniske beskrivelser av hvordan
  vinduer skal settes sammen). Noen vinduer har ventiler, men plassering av
  ventil avhenger av hvilken vindustype det er 
- alle vindustyper har sin måte å beregne på, lager en klasse for hver måte
  (hver sin algoritme), hver av disse er en Strategy (VentilStrategy). Velge
  runtime hvilken som er riktig
- Hva er Strategy?
- mye tilsvarende til Factory, men for algoritmer
- Hvilket problem løser Strategy: 
- skille virkemåten til en algoritme fra hvordan den brukes
- frikoble valg og bruk av algoritme fra implementasjonen
- når du må bestemme hvilken type algoritme som skal løse et problem runtime
- kunne laget alle algoritmene i en stor if-else med metoder for de ulike
  algoritmene, men det er vanskelig å vedlikeholde over tid (og bryter med
  open/closed-principle)
- Hvordan implementerer vi Strategy?
- Lag et interface, en Strategy, alle klasser implementerer denne. Velg runtime
  hvilken klasse som skal instansieres, mens resten av koden bare forholder seg
  til interface-metodene. 
- Ref eksempelet over: hva hvis det ikke skal være noen ventil? Kan lage Null
  Object


---

### Null object

Note:
- designmønster som definerer ingen-oppførsel
- Hvis vindu ikke skal ha ventil, så kan det lages en Strategy-implementasjon
  for ingen ventil
- blir en egen implementasjonsklasse som da gjerne ikke gjør noe beregning eller
  på annet vis gir en bra (og definert!) behandling av hva som skal skje i
  ikke-tilfellet
- særlig i Java er null et "eget konsept" som ikke egentlig er et objekt, men
  som kan returneres i stedet for ett. Null object er en vei rundt dette, for å
  gi bedre håndtering i kode av null. 
- hva betyr null? Er det en feil eller er det ok? Må håndteres alle steder det
  objektet hentes i koden (bryter flere SOLID-prinsipper). 


---

### Structural patterns

Note: 
- handler om å få kommunikasjon mellom ulike entiteter til å bli enklere


---

### Adapter

Note:
- Legge et nytt lag med programvare for å få konsepter (klasser) med ulike
  tenkemåter til å kunne fungere på samme måte 
- tenk: ulike typer strømadaptere, må legge noe i mellom for å kunne bruke dine
  elektriske duppedingser i ulike land med ulike standarder
- hvilket problem løser Adapter:
- lar kode som allerede er laget virke sammen sømløst for klienten sin del. 
- isolerer kompabilitetsendringer og gjør det lettere for alle andre
- viser at ulike konsepter er relaterte og kan brukes om hverandre (selv om de
  feks kommer fra ulike biblioteker)
- Eksempel: samme gui-logikk for ulike gui-rammeverk: får en Adapter definerer
  felles oppførsel, hver implementasjon limer sammen felles funksjonalitet med
  detaljer fra hver rammeverk
- Også mye brukt i testing for å injisere tester der det finnes funksjonalitet
  som ikke under noen omstendigheter må kalles
- Eksempel: 
- Testing av atomvåpen-software: vi har ikke lyst til å faktisk sende avgårde et
  atomvåpen, men vi må teste at systemet virker
