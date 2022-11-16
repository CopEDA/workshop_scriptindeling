Doel: script om voorbeeld van script-structuur te laten zien
Subdoel: automatisch verwerken van aangeboden data tot grafieken
Gemaakt door Bastiaen Boekelo en Ciska Overbeek, zomer 2022
Output is overzicht van grafieken per locatie en parameter, voor 4 specifieke parameters en 4 specifieke locaties voor de KWA van HDSR. 


Parameters:
WNS3882, chloride = [] [CONCTTE] [Cl] [mg/l] [nf] [OW]
WNS7128, Geleidendheid = [] [GELDHD] [] [uS/cm] [25oC] [OW]
WNS1923, Temperatuur = [] [T] [] [oC] [NVT] [OW]
WNS9943, Bemonsteringsdiepte = [] [MONSDTE] [] [m] [NVT] [OW]

Locaties:
NL14_20094: e04 Doorslag Hollandsche IJssel,133859,447937
NL14_20112: e26 Polderwetering gemaal de Koekoek,123546,441458
NL14_20906: d32 Leidsche Rijn gemaal de Aanvoerder,133563,454951
NL14_20930: w25 Inlaat Hekendorp,114907,447493

Een overzicht van de meetlocaties is te bekijken in 'Kaart_KWA_Meetpunten.pdf'

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
WAT MOET JE DOEN


STAP 1: Zet bestanden klaar
- Exporteer de juiste bestanden uit LIMS. Let op dat de juiste parameters en locaties zijn geselecteerd, dit gaat het gemakkelijkst via Order.number.
- Plaats deze csv bestanden in de folder input.
- Als je een file wil vervangen kun je hem deleten of naar de folder '__oud__' verplaatsen

STAP 2: Run script
- Open het R-project 'workshop_scriptindeling.Rproj'
- Open hierin het R-script 'voorbeeld_scriptindeling.R'
- Selecteer alle regels
- Druk Ctrl-Enter
- Sluit R af
- Open de folder 'output'
- Als het goed is gegaan zijn er hier twee pdf's aangemaakt: een met en een zonder kaart.

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
VERSION CONTROL


Deze map kan worden gesynchroniseerde met Github (https://github.com/CoPEDA/workshop_scriptindeling.git)
Hier zou de laatste versie aanwezig moeten zijn.

Voor vragen over deze folder, dit script of het versiebeheer, neem contact op met ciska.overbeek@hdsr.nl




