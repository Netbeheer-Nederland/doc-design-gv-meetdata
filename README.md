# Inleiding

Dit ontwerp beschrijft de beoogde gebruiksscenario's voor het dataproduct *GV
Meetdata*, alsmede de achterliggende architectuur en gemaakte keuzes.

# Reikwijdte

Er is steeds meer behoefte aan inzicht in data over energieverbruik en
energieopwekking van grootverbruikers (GV) om optimaal gebruik te maken van de
beschikbare energiecapaciteit. Deze data is bijvoorbeeld relevant voor
bedrijven die willen samenwerken op een industrieterrein of voor organisaties
die individuele bedrijven willen helpen met duurzame energie oplossingen en
energiebesparingen. De GV-klant is eigenaar van deze data en moet toestemming
geven aan andere partijen om de data te gebruiken. Een erkend
meetverantwoordelijke (MV) verzorgt de metingen van deze data.

Dit ontwerp beschrijft het proces om tot ontsluiting van het bijbehorende
dataproduct te komen en de betekenis & structuur van het dataproduct.

# Achtergrond

Onderstaand wordt kort de achtergrond toegelicht m.b.t. dataproducten.

## Dataproduct

Voor alle data-uitwisseling binnen de scope van de visie is expliciet gekozen
voor het denken over data in de vorm van dataproducten. Een dataproduct
combineert de semantische-, technische- en gebruiksaspecten van
data-uitwisseling. Om dit invulling te geven bestaat een dataproduct uit de
volgende componenten:

* *dataset*: de daadwerkelijke gegevens die worden uitgewisseld. Zie een
  dataset als een tabel met gegevens, waarbij de kolommen beschrijven wat er op
  elke rij van de data aan gegevens wordt geleverd. Een dataset voor
  aansluitingen zal minstens een kolom "Aansluitingsnummer" bevatten, waarbij
  elke rij in de dataset een aansluiting beschrijft;
* *dataservice*: de technische manier van verspreiden van de dataset. Dit gaat
  over hoe de data ontsloten wordt (als bestand, API, database of *file
  server*);
* *voorwaarden*: er kunnen voorwaarden liggen op beschikbaarheid, kwaliteit,
  classificatie en doelbinding bij gebruik van het dataproduct.

Het dataproduct combineert de *dataset* en *dataservice*, verrijkt met
*voorwaarden* voor gebruik.

## Volume, variety, veracity, velocity

Data kent vier kenmerken:

* *volume*: data kent een volume, een hoeveelheid;
* *variety* (variëteit): data is te verdelen in gestructureerde en
  ongestructureerde data. Ongestructureerde data kent geen metamodel;
* *veracity* (betrouwbaarheid): de mate waarin de data vertrouwd kan worden
  voor de toepassing;
* *velocity* (snelheid): de frequentie waarmee data verandert.

# Functionele eisen

Het dataproduct is onderheving aan functionele eisen.

| Als          | Wil ik | Want                                                |
|--------------|--------|-----------------------------------------------------|
| Netbeheerder | Een geldigheid van 90 dagen op een verleende toestemming | Ik beperk het aantal toestemmingverzoeken. |

# Dataservice

In beide gebruiksscenario's vraagt de GV-klant of een *derde* (zoals een
adviesbureau of ontwikkelbedrijf) meetgegevens op bij de netbeheerder van een
GV-klant. Indien een derde meetgegevens opvraagt dient toestemming verleend te
worden door de aangeslotene. Hierbij vervult de GV-klant de rol van
*aangeslotene*.

> [!IMPORTANT]
> Beide gebruiksscenario's gaan uit van *geauthenticeerde rollen*. Voor
> authenticatie dient aangesloten te worden op bestaande processen. Indien er
> geen *enrolment- en authenticatieproces* is *moet* deze worden ingericht.

De gebruiksscenario's beschrijven niet het uitvalproces (*happy flow*), deze
stappen dienen apart beschreven en geïmplementeerd te worden.

## Gebruiksscenario: centraal dataproduct

![Gebruiksscenario](assets/use_case_01-20241125.svg)

1. een *GV-klant* of een *derde* (e.g. een ontwikkelbedrijf) vraagt
   meetgegevens op voor één of meerdere aansluitingen bij het *EDSN
   meetdataregister*.  GV-klanten vragen meetdata aan voor eigen
   aansluiting(en), derden voor te specificeren aansluitingen;
2. het meetdataregister toetst of er toestemming is verleend voor de combinatie
   derde/aansluiting. Indien de GV-klant meetdata voor eigen aansluiting(en)
   aanvraagt is toestemming niet vereist en wordt de meetdata geleverd (stap
   6);
3. indien er geen toestemming is voor opvragen van de meetdata wordt de
   *aangeslotene* gevraagd deze te verlenen. De aangeslotene kan potentieel
   weken wachten voordat toestemming verleend of afgewezen wordt;
4. de verleende toestemming wordt vastgelegd in het Toestemmingplatform;
5. het Toestemmingplatform notificeert het meetdataregister dat toestemming
   verleend is door de aangeslotene;
6. het meetdataregister levert de meetgegevens, onder het *uniforme schema*.

Het beschreven scenario kent een aantal varianten voor implementatie:

* de netbeheerder leidt *GV-klanten* vanuit de *Mijn-omgeving* door naar het
  meetdataregister;
* de netbeheerder leidt *derden* door naar het meetdataregister;
* de netbeheerder ontsluit het meetdataregister vanuit de Mijn-omgeving.
  Hiervoor vraagt de netbeheerder onder eigen autorisatie de meetgegevens op
  voor de geauthenticeerde GV-klant;
* de netbeheerder ontsluit het meetdataregister vanuit de partner- of
  derdenomgeving. Een derde vraagt voor specifieke aansluiting(en) meetgegevens
  op, welke na toestemming van de aangeslotene worden geleverd. Niet alle
  netbeheerders kennen een dergelijke partneromgeving.

## Gebruikersscenario: decentraal dataproduct

![Gebruiksscenario](assets/use_case_02-20241125.svg)

1. een *GV-klant* of een *derde* (e.g. een ontwikkelbedrijf) vraagt
   meetgegevens op voor één of meerdere aansluitingen bij de netbeheerder.
   GV-klanten vragen meetdata aan voor eigen aansluiting(en), derden voor te
   specificeren aansluitingen;
2. de netbeheerder toetst of er toestemming is verleend voor de combinatie
   derde/aansluiting. Indien de GV-klant meetdata voor eigen aansluiting(en)
   aanvraagt is toestemming niet vereist en wordt de meetdata geleverd (stap
   5);
3. indien er geen toestemming is voor opvragen van de meetdata wordt de
   *aangeslotene* gevraagd deze te verlenen. De aangeslotene kan potentieel
   weken wachten voordat toestemming verleend of afgewezen wordt;
4. de verleende toestemming wordt vastgelegd in de administratie van de
   netbeheerder;
5. de netbeheerder levert de meetgegevens, onder het *uniforme schema*.

Het beschreven scenario kent een aantal varianten voor implementatie:

* de netbeheerder levert meetdata als voor de *GV-klant* vanuit de *Mijn-omgeving*;
* de netbeheerder levert meetdata als *maatwerkverzoek* vanuit de eigen *service
  desk*, of gedelegeerd naar *Partners in Energie*.

## Architectuur

Vanuit architectuurperspectief zijn de volgende keuzes gemaakt:

![Architectuur](assets/architecture-20241118.svg)

* de structuur (logisch model) van de dataset is gebaseerd op het [conceptuele
  model Begrippenmodel NBNL](https://begrippen.netbeheernederland.nl/) en het
  [Common Information Model](https://cim-mg.ucaiug.io/latest/);
* het dataproduct *GV Meetdata* is vindbaar via het EDSN Dataportaal.

# Dataset

## Volume, variety, veracity, velocity

| Type     | Beschrijving                                                     |
|----------|------------------------------------------------------------------|
| Volume   | Laag, < 100MB (ongecomprimeerd).                                 |
| Variety  | Gestructureerd.                                                  |
| Veracity | Hoog, de dataset wordt samengesteld uit brongegevens die ook voor facturatie worden ingezet. |
| Velocity | Laag, datasets worden op verzoek samengesteld.                   |

# Voorwaarden

Dit dataproduct wordt als [gesloten
data](https://github.com/Netbeheer-Nederland/am-doelarchitectuur-datadelen/blob/cebef14d35eaedd808cf9cb9ec7e931d0c5178c3/assets/20230217_NBNL_T5_Visie%20op%20datadelen_V1.01.pdf)
aangeboden, onder de [Grondslag
toestemming](https://www.autoriteitpersoonsgegevens.nl/themas/basis-avg/avg-algemeen/grondslag-toestemming).
De grondslag is vereist indien een derde de aanvraag doet, voor een GV-klant is
toestemming **niet** vereist indien het om meetdata van de eigen
aansluiting(en) gaat.

# Beslissingen en aannames

| Type       | Beschrijving                                                   |
|------------|----------------------------------------------------------------|
| Aanname    | Netbeheerders hebben de gevraagde meetgegevens (dataset) beschikbaar voor levering als decentraal dataproduct. |
| Aanname    | EDSN maakt geen aanpassingen in de bestaande interface(s) voor het decentrale dataproduct. |
| Aanname    | De aangeslotene verleent toestemming over toegang tot *al* haar meetdata behorende bij een aansluiting. Dit voorkomt toestemmingverzoeken voor meetgegevens in een tijdsperiode. |
| Aanname    | Zowel GV-klant, als aangeslotene, als derde zijn te identificeren, authenticeren en te benaderen voor communicatie. |
| Aanname    | Toestemming wordt verleend op basis van de combinatie derde/aansluiting. |
| Aanname    | *Partners in Energie* stopt met het invullen van maatwerkverzoeken voor meetdata na implementatie van het centrale dataproduct. |
