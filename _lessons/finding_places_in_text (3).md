---
layout: page
title: (NOT WORKING) Finding Places in Text with the World Historical Gazeteer
description: This Programming Historian lesson has taught me about the resources available to digital historians that allow them to learn about geographical locations mentioned throughout texts, and further, to visualize such locations for reconciliation and geocoding purposes. 
---

### Finding Places in Text with the World Historical Gazeteer
Kevin Arellano Flores

CLS-0161 -- Introduction to Digital Humanities

Professor Saxton


```python
text = "Siberia has many rivers."

# Using enumerate() method to locate characters along text, by index.
for index, char in enumerate(text):
    print(index, char)
```

    0 S
    1 i
    2 b
    3 e
    4 r
    5 i
    6 a
    7  
    8 h
    9 a
    10 s
    11  
    12 m
    13 a
    14 n
    15 y
    16  
    17 r
    18 i
    19 v
    20 e
    21 r
    22 s
    23 .



```python
# Using find() method to locate sequence of characters along text.
index = text.find("rivers")

print(f'"rivers" was located at index {index}.')
```

    "rivers" was located at index 17.



```python
# Using find() method to display emphasis on lower/upper case spelling of terms.
index = text.find("Rivers")

print(f'"Rivers" was located at index {index}.')
```

    "Rivers" was located at index -1.



```python
# Using find() method to display how it seeks only to identify exact matches for character sequences.
index = text.find("y riv")

print(f'"y riv" was located at index {index}.')
```

    "y riv" was located at index 15.



```python
pip install spacy
```

    Requirement already satisfied: spacy in /Users/kevin/opt/anaconda3/lib/python3.9/site-packages (3.4.1)
    Requirement already satisfied: preshed<3.1.0,>=3.0.2 in /Users/kevin/opt/anaconda3/lib/python3.9/site-packages (from spacy) (3.0.7)
    Requirement already satisfied: pathy>=0.3.5 in /Users/kevin/opt/anaconda3/lib/python3.9/site-packages (from spacy) (0.6.2)
    Requirement already satisfied: requests<3.0.0,>=2.13.0 in /Users/kevin/opt/anaconda3/lib/python3.9/site-packages (from spacy) (2.27.1)
    Requirement already satisfied: wasabi<1.1.0,>=0.9.1 in /Users/kevin/opt/anaconda3/lib/python3.9/site-packages (from spacy) (0.10.1)
    Requirement already satisfied: srsly<3.0.0,>=2.4.3 in /Users/kevin/opt/anaconda3/lib/python3.9/site-packages (from spacy) (2.4.4)
    Requirement already satisfied: spacy-loggers<2.0.0,>=1.0.0 in /Users/kevin/opt/anaconda3/lib/python3.9/site-packages (from spacy) (1.0.3)
    Requirement already satisfied: typer<0.5.0,>=0.3.0 in /Users/kevin/opt/anaconda3/lib/python3.9/site-packages (from spacy) (0.4.2)
    Requirement already satisfied: spacy-legacy<3.1.0,>=3.0.9 in /Users/kevin/opt/anaconda3/lib/python3.9/site-packages (from spacy) (3.0.10)
    Requirement already satisfied: tqdm<5.0.0,>=4.38.0 in /Users/kevin/opt/anaconda3/lib/python3.9/site-packages (from spacy) (4.64.0)
    Requirement already satisfied: pydantic!=1.8,!=1.8.1,<1.10.0,>=1.7.4 in /Users/kevin/opt/anaconda3/lib/python3.9/site-packages (from spacy) (1.9.2)
    Requirement already satisfied: numpy>=1.15.0 in /Users/kevin/opt/anaconda3/lib/python3.9/site-packages (from spacy) (1.21.5)
    Requirement already satisfied: cymem<2.1.0,>=2.0.2 in /Users/kevin/opt/anaconda3/lib/python3.9/site-packages (from spacy) (2.0.6)
    Requirement already satisfied: packaging>=20.0 in /Users/kevin/opt/anaconda3/lib/python3.9/site-packages (from spacy) (21.3)
    Requirement already satisfied: catalogue<2.1.0,>=2.0.6 in /Users/kevin/opt/anaconda3/lib/python3.9/site-packages (from spacy) (2.0.8)
    Requirement already satisfied: langcodes<4.0.0,>=3.2.0 in /Users/kevin/opt/anaconda3/lib/python3.9/site-packages (from spacy) (3.3.0)
    Requirement already satisfied: thinc<8.2.0,>=8.1.0 in /Users/kevin/opt/anaconda3/lib/python3.9/site-packages (from spacy) (8.1.3)
    Requirement already satisfied: setuptools in /Users/kevin/opt/anaconda3/lib/python3.9/site-packages (from spacy) (61.2.0)
    Requirement already satisfied: jinja2 in /Users/kevin/opt/anaconda3/lib/python3.9/site-packages (from spacy) (2.11.3)
    Requirement already satisfied: murmurhash<1.1.0,>=0.28.0 in /Users/kevin/opt/anaconda3/lib/python3.9/site-packages (from spacy) (1.0.8)
    Requirement already satisfied: pyparsing!=3.0.5,>=2.0.2 in /Users/kevin/opt/anaconda3/lib/python3.9/site-packages (from packaging>=20.0->spacy) (3.0.4)
    Requirement already satisfied: smart-open<6.0.0,>=5.2.1 in /Users/kevin/opt/anaconda3/lib/python3.9/site-packages (from pathy>=0.3.5->spacy) (5.2.1)
    Requirement already satisfied: typing-extensions>=3.7.4.3 in /Users/kevin/opt/anaconda3/lib/python3.9/site-packages (from pydantic!=1.8,!=1.8.1,<1.10.0,>=1.7.4->spacy) (4.1.1)
    Requirement already satisfied: charset-normalizer~=2.0.0 in /Users/kevin/opt/anaconda3/lib/python3.9/site-packages (from requests<3.0.0,>=2.13.0->spacy) (2.0.4)
    Requirement already satisfied: urllib3<1.27,>=1.21.1 in /Users/kevin/opt/anaconda3/lib/python3.9/site-packages (from requests<3.0.0,>=2.13.0->spacy) (1.26.9)
    Requirement already satisfied: idna<4,>=2.5 in /Users/kevin/opt/anaconda3/lib/python3.9/site-packages (from requests<3.0.0,>=2.13.0->spacy) (3.3)
    Requirement already satisfied: certifi>=2017.4.17 in /Users/kevin/opt/anaconda3/lib/python3.9/site-packages (from requests<3.0.0,>=2.13.0->spacy) (2021.10.8)
    Requirement already satisfied: blis<0.8.0,>=0.7.8 in /Users/kevin/opt/anaconda3/lib/python3.9/site-packages (from thinc<8.2.0,>=8.1.0->spacy) (0.7.8)
    Requirement already satisfied: confection<1.0.0,>=0.0.1 in /Users/kevin/opt/anaconda3/lib/python3.9/site-packages (from thinc<8.2.0,>=8.1.0->spacy) (0.0.3)
    Requirement already satisfied: click<9.0.0,>=7.1.1 in /Users/kevin/opt/anaconda3/lib/python3.9/site-packages (from typer<0.5.0,>=0.3.0->spacy) (8.0.4)
    Requirement already satisfied: MarkupSafe>=0.23 in /Users/kevin/opt/anaconda3/lib/python3.9/site-packages (from jinja2->spacy) (2.0.1)
    Note: you may need to restart the kernel to use updated packages.



```python
# Loading German language support from spaCy NLP library into a "language object" (nlp).
from spacy.lang.de import German
nlp = German()
```


```python
# Tokenization of text in German language.
doc = nlp("Berlin ist eine Stadt in Deutschland.")

# Iteration of German text in effort to print token indices and associated text 
for token in doc:
    print(token.i, token.text)
```

    0 Berlin
    1 ist
    2 eine
    3 Stadt
    4 in
    5 Deutschland
    6 .



```python
# Importing pathlib library.
from pathlib import Path

# Using pathlib to process a text file to be read.
gazetteer = Path("gazetteer.txt").read_text()

# Making a list of location names by splitting the text file by '\n'.
gazetteer = gazetteer.split('\n')

# Should print list of location names.
print(gazetteer)
```

    ['Armenien', 'Aserbaidshan', 'Aserbaidshen', 'Estland', 'Georgien', 'Kasachstan', 'Kirgisien', 'Lettland', 'Litauen', 'Moldawien', 'Russland', 'RSFSR', 'Kazakhstan', 'Turkmenien', 'Usbekistan', 'Ukraine', 'Weißrussland', 'Weissrussland', 'Abchasien', 'Akmola', 'Aktjubinsk', 'Alma Ata', 'Gurjew', 'Karaganda', 'Kostai', 'Ostkasachstan', 'Sudkasachstan', 'Siidkasachstan', 'DshalaLAbad', 'Frunse', 'Osch', 'Basarabeasca', 'Adygejien', 'Altai', 'Archangelsk', 'Astrachan', 'Baschkirien', 'Brjansk', 'Burjatien', 'Dagestan', 'Gorki', 'Gorkif Tschkalowsk', 'Irkutsk', 'Iwanowo', 'Jaroslawl', 'KabardinienBalkarien', 'Kalinin', 'Kaliningrad', 'Kalmykien', 'Kaluga', 'KaratschaiTscherkessien', 'Karelien', 'Kemerowo', 'Kislar', 'Kingissepp', 'Kirow', 'Komi', 'Krasnodar', 'Krasnoyarsk', 'Krim', 'Kuibyschew', 'Kurgan', 'Kursk', 'Leningrad', 'Marij El', 'Molotow', 'Mordowien', 'Moskau', 'Murmansk', 'Nordossetien', 'Nowgorod', 'Nowosibirsk', 'Omsk', 'Ordshonikidse', 'Orjol', 'Oijol', 'Pensa', 'Perm', 'Primorje', 'Pskow', 'Rjasan', 'Rostow am Don', 'Saratow', 'Smolensk', 'Stalingrad', 'Stawropol', 'Swerdlowsk', 'Tambow', 'Tatarstan', 'Tjumen', 'Tula', 'T ula', 'Udmurtien', 'Uljanowsk', 'Tscheljabinsk', 'Tscheijabinsk', 'Tscherkessien', 'TschetschenoInguschetien', 'Tschkalow', 'Tschuwaschien', 'Wladimir', 'Wologda', 'Woronesh', 'Aschchabad', 'Andishan', 'Ferga', 'Taschkent', 'Taschkentj', 'Charkow', 'Chmelnizki', 'Dnepropetrowsk', 'Dnepropetrovsk', 'Drogobytsch', 'KamenezPodolski', 'Kiew', 'Kirowograd', 'Lwow', 'Nikolajew', 'Odessa', 'Poltawa', 'Rowno', 'Saporoshje', 'Shitomir', 'Stalino', 'Stanislaw', 'Sumy', 'Su my', 'Tschernigow', 'Winniza', 'Wolynien', 'Woroschilowgrad', 'Gomel', 'Grodno', 'Minsk', 'Mogiljow', 'Pinsk', 'PoLessje', 'Polozk', 'Wilejka', 'Witebsk', 'Alawerdi', 'Arthik', 'Jerewan', 'Oktemberjan', 'Sewan', 'Talin', 'Chanlar', 'Chilly', 'Daschkesan', 'Nucha', 'Samuch', 'Stadt Baku', 'Stadt Kirowabad', 'Harjumaa', 'MorskoiStadtbezirk Tallinn', 'Paide', 'Parnu', 'Raplamaa', 'Valga', 'Otschamtschira', 'Agbulach', 'Macharadse', 'Tbilissi', 'ZaLendshicha', 'Zchaltubo', 'Wischnjowka', 'TaLdykorgan', 'Makat', 'BalchaschStadt', 'Karsakpai', 'Leninogorsk', 'Syrjanowsk', 'Lenger', 'Pachtaaral', 'Turkestan', 'TaschKumyr', 'Kemin', 'Bauska', 'Cesis', 'Jekabpils', 'Jelgava', 'Liepaja', 'Ogre', 'Riga', 'Saldus', 'Talsi', "Talsi'", 'Tukums', 'Akmene', 'Kaus', 'Siauliai', 'Vilkaviskis', 'Kischinjow', 'Vadul Lui Voda', 'BaruL', 'Sorokino', 'Troizkoje', 'Krasnoborsk', 'Oneshski', 'Plessezk', 'Primorski', 'Wolodarski', 'Belorezk', 'Birsk', 'Djatkowo', 'wlja', 'Potschep', 'Susemka', 'Trubtschewsk', 'Iwolginsk', 'PribaikalskiKreis', "Sa'igrajewo", "Sai'grajewo", 'Selenga', 'Tarbagatai', 'Buiksk', 'Balach', 'Bogorodsk', 'Bor', 'Dsershinsk', 'Linda', 'Kulebaki', 'waschino', 'Schachunja', 'Tonschajewo', 'Tschistoje', 'Uren', 'Worotynez', 'Sljudjanka', 'Gawrilow Possad', 'Jurjewez', 'Jusha', 'Kirshatsch', 'Leshnewo', 'Palech', 'Sereda', 'Tejkowo', 'Brejtowo', 'Nerechta', 'PereslawlSalesski', 'UgLitsch', 'ltschik', 'Firowo', 'Mednoje', 'Rshew', 'Sawidowo', 'Spirowo', 'Torshok', 'Wyschni Wolotschok', 'Lagan', 'Wyssokinitschi', 'MikojanKreis', 'UstDsheguta', 'Belomorsk', 'Kalewaly', 'Medweshjegorsk', 'Pitkaranta', 'Prioneshski', 'Pudosh', 'Segesha', 'Wolossowo', 'Kai', 'Kilmes', 'Werchnekamski', 'Wjatskije Poljany', 'Knjashpogost', 'UstWym', 'Beloretschensk', 'Gelendshik', 'Krasnoarmejskaja', 'Kuschtschewskaja', 'Neftegorsk', 'Pawlowskaja', 'Sewerskaja', 'Sowetskaja', 'Uspenskoje', 'Aluschta', 'Balaklawa', 'Bachtschissarai', 'Jalta', 'JaLtaStadt', 'KarassuBasar', 'Kolai', 'Krasnogwardejskoje', 'Krasnoperekopsk', 'MajakSalynski', 'Saki', 'Seitlerski', 'SewastopolStadt', 'Simferopol', 'Stary Krym', 'Sudak', 'Suja', 'MolotowKreis', 'Schigony', 'Shiguijowsk', 'GLuschkowo', 'Mikojanowka', 'Schebekino', 'Borowitschi', 'Ljubytino', 'Lodejnoje Pole', 'Malaja Wischera', 'Okulowka', 'Opetschenski', 'Pargolowo', 'Pestowo', 'Podporoshje', 'Sluzk', 'Tichwin', 'Utorgosch', 'Wolchow', 'Wsewoloshsk', 'JoschkarOla', 'Jurino', 'Kilemary', 'KiseLStadt', 'Kungur', 'MolotowStadt', 'Ochansk', 'Solikamsk', 'TschussowoiStadt', 'Subowa Polja', 'Temnikow', 'Balaschicha', 'Istra', 'Kaschira', 'Kolom', 'Krasnogorsk', 'Krasja Pachra', 'Krasja Polja', 'Kriwandino', 'Leninski', 'Moshaisk', 'Mytischtschi', 'roFominsk', 'Nowopetrowskoje', 'Osjory', 'Reutow', 'Reutov. Kutschino', 'Rusa', 'Schatura', 'Schtscholkowo', 'Solnetschnogorsk', 'StalinogorskStadt', 'Swenigorod', 'Uchtomski', 'Uwarowka', 'Wolokolamsk', 'Kirowsk', 'Kola', 'Dargkoch', 'Digora', 'AnsheroSudshensk', 'Assino', 'Gurjewsk', 'Jurga', 'Kusedejewo', 'Taiga', 'Taschtagol', 'Tjashinski', 'Apasjenkowskoje', 'Gorjatschewodski', 'Kursawka', 'Nowoalexandrowsk', 'Petrowskoje', 'Komaritschi', 'Urizki', 'Bolschoi Wjass', 'Kondolj Kondol', 'Mokschan', 'Gdow', 'Dankow', 'Kawerino', 'Lebedjan', 'Lew Tolstoi', 'Rybnoje', 'SoLotschinski', 'SpassKLepiki', 'Belaja Kalitwa', 'Matwejew Kurgan', 'NowoschachtinskStadt', 'Oktjabrski', 'Remontnoje', 'Simowniki', 'Swerjewo', 'Atkarsk', 'Kamenka', 'Krasnopartisansld', 'Krasny Kut', 'Rtischtschewo', 'Tatischtschewo', 'Woroschilowsk', 'Gshatsk', 'Isdeschkowo', 'Jarzewo', 'Krasny', 'Tumanowo', 'Balyklej', 'Frolowo', 'Gorodischtsche', 'Kotelnikowo', 'Krasja Sloboda', 'Georgijewsk', 'Isobilny', 'Suworowskaja', 'Alapajewsk', 'Aramil', 'AsbestStadt', 'AsbestĀ« Stadt', 'BerjosowskiStadt', 'Iss', 'Iwdel', 'Jegorschino', 'KirowgradStadt', 'Kirowgrad', 'Krasnoufimsk', 'Newjansk', 'Nishnjaja Saida', 'Nishnije Sergi', 'Nowaja Ljalja', 'PoLewskoi', 'Resh', 'RewdaStadt', 'Sysert', 'Tugulym', 'Werchnjaja Tura', 'Bondjushski', 'Kukmor', 'Nurlaty', 'Takanysch', 'Sawodoukowsk', 'Alexin', 'BogorodizkStadt', 'Bolochowo', 'Donskoi', 'Jepifan', 'Kimowsk', 'Laptewo', 'Mordwess', 'Schtschokino', 'Towarkowski', 'Tschern', 'TulaStadt', 'Uslowaja', 'Pudem', 'Uwa', 'Tscherdakly', 'Jetkul', 'Kussa', 'Minjar', 'Satka', "Atagi'", 'Atschaluki', 'AtschchoiMartan', 'dteretschnoje', 'Prigorodny', 'UrusMartan', 'Busuluk', 'KosLowka', 'GusChrustalny', 'Kameschkowo', 'Kurlowski', 'Sudogda', 'Wjasniki', 'Tschagoda', 'Tscherepowez', 'UstKubinski', 'Grjasi', 'JelanKolenowski', 'Dshalalkuduk', 'Isbaskan', 'Begowat', 'Tschis', 'Tschirtschik', 'IBogoduchow', 'Kegitschowka', 'Nowaja Wodolaga', 'Polonnoje', 'Baryschewka', 'Beresan', 'Christinowka', 'Jagotin', 'Alexandrowka', 'Malaja Wiska', 'Busk', 'Nowy Bug', 'Gaiworon', 'Gruschka', 'Globino', 'Lochwiza', 'Genitschesk', 'Michajlowka', 'Berditschew', 'PopoLnja', 'Charzyssk', 'GorLowkaStadt', 'Krasnoarmejsk', 'MakejewkaStadt', 'Selidowo', 'Jampot', 'Nedrigaitow', 'Utjanowka', 'Neshin', 'PriLuki', 'Litin', 'Turbow', 'Tywrow', 'Perwomaisk Stadt', 'Krasny Lutsch Stadt', 'Uwarowitschi', 'WoLkowysk', 'Puchowitschi', 'Saslawl', 'Beresino', 'Bobruisk', 'Ganzewitschi', 'Orscha', 'Sirotino', 'Ejlar', 'Armlu', 'Ararat', 'Etschmiadsin', 'Molotja', 'Kirowakan', 'Werchni Talin', 'Tumanjan', 'Alabaschly', 'Bajan', 'Baku', 'Kugitschu', 'Nishni Daschkesan', 'Werchni Daschkesan', 'Karadag', 'Kirowobad', 'Kirowabad', 'Kysyldsha', 'Kyrychly', 'Mingetschewir', 'Quba', 'Sabuntschi', 'Saljan', 'Schemacha', 'Sych', 'Adshikent', 'Sumgait', 'Ahtme', 'Loksa', 'Johvi', 'Kivioli', 'KohtlaJarve', 'Kuttejou', 'Lehtse', 'Maardu', 'Narva', 'JarvaJaani', 'Lavassaare', 'Rakvere', 'Jarvakandi', 'Sonda', 'Tallinn', 'Tammiku', 'Tapa', 'Tartu', 'Ulila', 'Loosi', 'Nudrezowo', 'Podra', 'Kingu', 'Vasalemma', 'Vontkula', 'Suchumi', 'Kelasuri', 'Miussera', 'Tkwartscheli', 'Manglissi', 'Awtschala', 'Bachmaro', 'Bergwerkssiedlung Nr. 2', 'Bolnissi', 'Bulutschauri', 'ChezerTeesowchos', 'ChramiWasser kraftwerkssiedlung', 'Dsegwi', 'Dwiri', 'Ingiri', 'Kluchori', 'Ksani', 'KutaĆÆssi', 'KutaTssi', 'Kutaiā€™ssi', 'Kutaissi', 'Kwareli', 'Sairme', 'Mukusani', 'wtLugi', 'Poti', 'Rustawi', 'Samgori', 'Sugdidi', 'Supsa', 'Barmaksis', 'Teegenossenschaft Didatschkon', 'Teegenossenschaft raseni', 'Tk', 'Tschaladidi', 'Tschubrin', 'Ureki', 'Wedjanka', 'Zulukidse', 'Atbassar', 'Jessil', 'Koluton', 'Ko Luton', 'Makinka', 'TekeLi', 'Baischoss', 'DshambuL', 'Agadyr', 'Ar', 'Kounradski', 'Bergwerksiedlung 2626 BIS', 'Bergwerksiedlung 4243', 'KaragandaSortirowotschja', 'Dsheskasgan', 'Kokusek', 'MaiKuduk', 'Nurinskaja', 'Saran', 'SpasskiWerkssiedlung', 'Temirtau', 'Dshetygara', 'Kuschmurun', 'Kysylorda', 'Irtyschgesstroi', 'Beloussowka', 'Jerofejewka', 'Syrjanowskoje', 'UstKamenogorsk', 'Syrdarjinskaja', 'Tschimkent', 'Atschissaj', 'Kantagi', 'KjokDshangak', 'DsheLAryk', 'KisKyja', 'Bystrowka', 'Maimak', 'KysylKyja', 'Suljukta', 'Auce', 'Iecava', 'LIgatne', 'Daugavpils', 'Dzukste', 'Eglaine', 'Jaunjelgava', 'Krustpils', 'Garoza', 'Misa', 'Turmali', 'Jugla', 'Kraslava', 'Kuldiga', 'Pavilosta', 'Satinskoje', 'LTgatne', 'Murjani', 'Kegums', 'Olaine', 'Priedaine', 'Purmsati', 'Rampa', 'Rezekne', 'Balozi', 'Salaspils', 'Stopini', 'Broceni', 'Sarkandaugava', 'Skulte', 'Sloka', 'Suntazi', 'Stende', 'Taurupe', 'T ukums', 'Valmiera', 'Ventspils', 'Bicionys', 'Gaideliai', 'Kaisiadorys', 'Ezerelis', 'Raudelino', 'Samaniai', 'Klaipeda', 'Kretinga', 'Mauruciai', 'ujoji Vilnia', 'Palemos', 'RadviLiskis', 'Taurage', 'Telsiai', 'Kybartai', 'Vilnius', 'Abaklia', 'Balti', 'Tiraspol', 'Orhei', 'Ungheni', 'Maikop', 'Tschesnokowka', 'Bisk', 'Blinowo', 'Rubzowsk', 'Newerowskaja', 'Sarinskaja', 'Schpagino', 'Smasnewo', 'Borowljanka', 'Jemza', 'Jerzewo', 'Jura', 'Kargopol', 'Kisema', 'Kostyljowo', 'Kotlas', 'Ustjewo', 'Molotowsk', 'Moschnoje', 'Njandoma', 'Oboserski', 'wolok', 'Podjuga', 'Solsa', 'Schalakuscha', 'Tschekamino', 'Welsk', 'Kapustin Jar', 'Nikolskoje', 'Perwomaiski', 'Tumanny', 'Werchni Baskuntschak', 'Wladimirowka', 'Wolodarowka', 'Beljagusch', 'Inser', 'Nukatowo', 'Sorwicha', 'Karlaman', 'Manjawa', 'Tschernikowka', 'Tschernikowsk', 'Ufa', 'Besymjanka', 'Bijansk', 'Brjansk1', 'Brjansl<2', 'Brjansk2', 'Bytosch', 'Iwot', 'Star', 'Zementny', 'Karatschew', 'Kletnja', 'KLinzy', 'Altuchowo', 'Nowosybkow', 'Ordshonikidsegrad', 'Palushje', 'Pogreby', 'Ramassucha', 'Selzo', 'Kokorewka', 'Belaja Berjoska', 'Unetscha', 'Wygonitschi', 'Dshida', 'Gorchon', 'Gorodok', 'Sawodskoi', 'JushlagSiedlung', 'Nowoiljinsk', 'Onochoi', 'Pedshino', 'Turka', 'Sagustai', 'Surgalej', 'Schabur', 'Werchnije Talzy', 'Saudinski', 'Gussinoje Osero', 'Oronchoj', 'Selenduma', 'Suchaja Padj', 'UlanUde', 'Werchnjaja Mojsa', 'Chabarowsk', 'Karamachi', 'Derbent', 'Gedshuch', 'Isberbasch', 'Machatschkala', 'Mamedkala', 'Rubas', 'Arja', 'Awarijny', 'Prawdinsk', 'Oranki', 'Kershenez', 'Orlowo', 'Bystrucha', 'Pyra', 'Gluchaja', 'Igumnowo', 'Kiselicha', 'Kolchosny', 'Korelka', 'Krasnenki', 'Manturowo', 'Mochowye gory', 'Mostyrka', 'Mursizy', 'Tjoscha', 'Obchod', 'Sjawa', 'Scharja', 'Schemanicha', 'Sejma', 'Serebrjanka', 'Suchobeswodnoje', 'Tschornoje', 'Usta', 'Michailowski', 'Uwarowskaja', 'Rasneshje', 'Wyksa', 'Baischoss NOTE: Error in book. Makat is in Kazakhstan', 'Sagis. NOTE: Error in book. Makat is in Kazakhstan. Book said RSFSR', 'Siedlung des Werks Nr. 39', 'Strecke IrkutskSljudjanka', 'Strecke PosolskUlanUde', 'Strecke SljudjankaWydrino', 'Listwjanka', 'Strecke WydrinoPosolsk', 'Taischet', 'Demidowo', 'Furmanow', 'Iwankowo', 'Michailowo', 'Mugrejewo', 'Talizy', 'Kineschma', 'Kochma', 'Komsomolsk', 'Kowrow', 'Petrowski', 'Rodniki', 'Schuja', 'Jakowlewskoje', 'Siedlung des SchalowZiegelwerkes', 'Textilny', 'Witschuga', 'Iwkino', 'Chmelniki', 'Gawrilow Jam', 'Jakschanga', 'Kostroma', 'Lorn', 'Neja', 'Kosmynino', 'Perebory', 'Pereslawl', 'Rogosinino', 'Ussolje', 'Petrowsk', 'PriwoLshje', 'Rodionowo', 'Rybinsk', 'Schestichino', 'Schtschelkanskoje Torfopredprijatije', 'Semibratowo', 'Silnizy', 'Suprotiwny', 'Tichmenewo', 'Tutajew', 'Tschernizyno', 'WauLowo', 'Wspolje', 'AksautGretscheski', 'Kenshe', 'Akademitscheskaja', 'Beshezk', 'Bologoje', 'Emmaus', 'Wassiljewski Moch', 'Kimry', 'Kokowo', 'Kuwschinowo', 'Leontjewo', 'Maksaticha', 'Malyschewo', 'rotschino', 'Nelidowo', 'Nowossokolniki', 'Ossetschenka', 'Ostaschkow', 'Poddubki', 'Ranzewo', 'Redkino', 'Konyschewo', 'Olenino', 'Selisharowo', 'Semzy', 'Wydropushsk', 'Staraja Toropa', 'Dmitrowskoje', 'Dumanowo', 'Tschernogubowo', 'Tschorny Dor', 'Udomlja', 'Welikije Luki', 'Krasnomaiski', 'Konigsberg', 'Bagration owsk', 'Gut Romitten', 'Gwardejsk', 'Insterburg', 'Kaukiemis', 'Metgethen', 'Pillau', 'Preu&ischEylau', 'Ragnit', 'Seckenburg', 'Tilsit', 'Trammen', 'Tschernjachowsk', 'Wehlau', 'UlanChol', 'Kanukowo', 'Fajansowaja', 'Kusmitschi', 'Ljudinowo', 'Malojaroslawez', 'Suchinitschi', 'DurowSowchos', 'Chumara', 'Tscherkessk', 'Washnoje', 'Malenga', 'Letneretschenski', 'Saliwy', 'Tegosero', 'Iljinskaja', 'Kepa', 'Kondopoga', 'Medweshja Gora', 'arlahti', 'Pai', 'Petrosawodsk', 'Harlu', 'Laskela', 'Suojarvi', 'Pjashijewa Selga', 'Derewjanka', 'Botschilowo', 'Kolowo', 'Kubowskaja', 'NowoStekljannoje', 'Pjalma', 'Porschta', 'Schala', 'Schalski', 'Tschernoretschenski', 'Petrowski Jam', 'Polga', 'Uchta', 'Uuksu', 'Jurga1', 'Prokopjewsk', 'Stalinsk', 'Kikerino', 'Ardaschi', 'Besboshnik', 'Fosforitja', 'Ima', 'Werchnekamskaja', 'Latyschski', 'Loiga', 'Ljangassowo', 'Omutninsk', 'Posdino', 'Slobodskoi', 'Sosnowka', 'Stalja', 'Suslowka', 'Sosimski', "N. BeLoi'rka", 'WoshajeL', 'Lemju', 'Sewsheldorlag', 'Syktywkar', 'Sheschart', 'Adler', 'Apa', 'Apscheronsk', 'Armawir', 'Chadyshensk', 'Dshugba', 'Gluchari', 'Jurjewka', 'Mogilnoje', 'Kropotkin', 'Krymsk', 'Mogilny', 'Abchasskaja', 'Chadyshenskaa', 'Noworossisk', 'Ilski', 'Sotschi', 'Tichorezk', 'Tuapse', 'BogotoL', 'Sowchos Kastel', 'Belbek', "Tai'r", 'Dshankoi', 'Feodossija', 'Inkerman', 'AiWa nil', 'Alupka', 'Foros', "Korei's", 'Liwadija', 'Oreanda', 'Partenit', 'Jewpatorija', 'Mariano', 'Kertsch', 'Sowchos Bolschewik', 'Bagerowo', 'BuLgak', 'Eschkine', 'Sowchos Tschoschty', 'Tamak', 'Sewastopol', 'Sevastopol', 'Katscha', 'Sowchos KaraKijat', 'Sirenj', 'Siwasch', 'Sowchos AiDani', 'Sowchos Krasny', 'Koktebel', 'Sowchos 1. Fiinfjahrplan', 'Otusy', 'Sowchos Krymskaja Rosa', "Ta'ir", 'Bachilowa Polja', 'Batraki', 'Jablonowy Owrag', 'Jablonewy Owrag', 'Kadej', 'Kinel', 'Krjash', 'Melekess', 'Otwashnoje', 'Nikolajewka', 'Pochwistnewo', 'Sabdowka', 'Petscherskoje', 'Solnoje', 'Solonez', 'Sysran', 'Schadrinsk', 'Belgorod', 'Blagodatenski', 'Derjugino', 'Fatesh', 'Tjotkino', 'Gotnja', 'ShurawLjowka', 'Obojanj', 'Ryschkowo', 'NowoTawoLshanka', 'Tros', 'Waluiki', 'Besborodowo', 'Boksitogorsk', 'Jegla', 'Schibotowo', 'Ustje', 'Chotzy', 'Dibuny', 'Ishora', 'Jedrowo', 'Johannes', 'Kakisalmi', 'Kaskowo', 'Koli', 'Kolpino', 'Kowanko', 'Krasja Mysa', 'Krasnoje Selo', 'Krestzy', 'Kronstadt', 'LadogaSee', 'Ljalizy', 'Komarowo', 'Swirstroi', 'Luga', 'Lungatschi', 'Bolschoje Saborowje', 'Mga', 'Neboltschi', 'ParachinoPoddubje', 'Opetschenski Posad', 'Pesotschny', 'Petrodworez', 'PikaLjowo', 'Washiny', 'Pontonja', 'Puschkin', 'Rautu', 'Rogawka', 'Rudnitschja', 'Sestrorezk', 'Shichaijowo', 'Shicharjowo', 'Antropschino', 'UstIshora', 'Spasskaja Polistj', 'Staraja Russa', 'Stroganowo', 'Swir', 'Taizy', 'Terebutenez', 'Terioki', 'Tosno', 'Tschudowo', 'Turgosch', 'Uglowka', 'Waldaj', 'Sjasstroj', 'Wolchowstroi', 'Wolchowstroi2', 'MorosowSiedlung', 'Rachja', 'Golowino', 'SusLonger', 'Kosmodemjansk', 'Wolshsk', 'Baskaja', 'Beresniki', 'Gremjatschinski', 'Kisel', 'Gubacha', 'Jushny Kospaschski', 'Polowinka', 'Sewerny Kospaschski', 'Uswa', 'WsewolodoWilwa', 'Krasnokamsk', 'Kurja', 'Lyswa', 'Lewschino', 'Obogatitel', 'Obogatitelja', 'JugoKamski', 'Owerjata', 'Ponysch', 'Borowsk', 'Kalijez', 'Tscherdyn', 'Tschussowskaja', 'Paschia', 'Tjoplaja Gora', 'UrizkiBergwerke', 'Potma', 'Saransk', 'Sweshenkaja', 'Baratjewo', 'Akinschino', 'Aprelewka', 'Baranowo', 'Barmino', 'Bogorodskoje', 'Bronnizy', 'Butowo', 'Chimki', 'Chlebnikowo', 'Cholschtschewiki', 'Chotkowo', 'Chowrino', 'Chrapunowo', 'Dmitrow', 'Dolgoprudny', 'Dorochowo', 'Dres', 'Dulewo', 'Elektrostal', 'Fill', 'Golizyno', 'Golutwin', 'Gubanowo', 'Gutschkowo', 'Nowojerusalimskaja', 'Iwantejewka', 'Jegorjewsk', 'KaganowitschWerke', 'Osherelje', 'Katuar', 'Kisseljowo', 'Klin', 'Schtschurowo', 'Kolotsch', 'Koptjewo', 'Kossino', 'Iljinskoje', 'Pawschino', 'Tuschino', 'GosplanLandsiedlung', 'Lunjowo', 'Krasny Stroitel', 'Krjukowo', 'Lebsino', 'Tscherjomuschki', 'Lianosowo', 'Ljuberzy', 'Ljublino', 'Lobnja', 'Lopasnja', 'Luchowizy', 'Jurlowo', 'Obljanischtschewo', 'Schalikowo', 'Moskworezkaja', 'Mosshinka', 'Textilschtschik', 'Noginsk', 'Nowogorsk', 'OrechowoSujewo', 'Ostankino', 'Otdych', 'Otschakowo', 'Perowo', 'Planerja', 'Podlipki', 'Podolsk', 'PokrowskojeStreschnjewo', 'PtscheLowodnoje', 'Puschkino', 'Kutschino', 'Reutowo', 'Reschetnikowo', 'Rumjanzewo', 'Tutschkowo', 'Saraisk', 'Sasonowo', 'Frjasino', 'Semjonowskoje', 'Serebijany Bor', 'Serpuchow', 'Silikatja', 'Snegiri', 'Sorewnowanie', 'Sofrino', 'Rshawka', 'Kijukowo', 'Bergwerkssiedlung 28', 'Stroika', 'Stupino', 'Tomilino', 'Tscherkisowo', 'Tscherusti', 'Tschuchlinka', 'Kraskowo', 'Lytkarino', 'Ugreschskaja', 'Gorbuny', 'Kisseljowka', 'Poretschje', 'Werbilki', 'Wolololamsk', 'Woskressensk', 'Wyssokowsk', 'Kandalakscha', 'Saschejek', 'Kildinstroi', 'Murmaschi', 'Ku', 'Montschegorsk', 'Nikel', 'Olenja', 'Padun', 'Pautscha', 'Werchnjaja Simba', 'Nekrylowo', 'Beslan', 'Kardshin', 'Ursdon', 'Dsaudshikau', 'Krasnowodsk', 'Sadon', 'Wosnessensk', 'Perniza', 'Novosibirsk', 'Abagurowski', 'Alambai', 'Jaja', 'Ansherskaja', 'Artyschta', 'Barabinsk', 'Belowo', 'Bergwerke Jushja', "Salai'r", 'Inskoi', 'Kisseljowsk', 'Kriwoschtschokowo', 'Schuschtalep', 'LeninskKusnezki', 'Myski', 'Nowokusnezk', 'Berdsk', 'Ossinnilci', 'Ossinniki', 'Jaschkino', 'Tatarsk', 'Tjashin', 'Tschulym', 'Ussjaty', 'Issilkul', 'Kulomsino', 'Diwnoje', 'Mineralnyje Wody', 'Newinnomyssk', 'NowoALexandrowka', 'Pjatigorsk', 'Prijutnoje', 'Belyje Berega', 'Fokino', 'Jelez', 'Oelez', 'Mzensk', 'Pogar', 'Polushje', 'Assejewskaja', 'Bednodemjanowsk', 'Tschaadajewka', 'Kusnezk', 'Nikolajewski chutor', 'Nishni Lomow', 'Seliksa', 'Simanschtschino', 'Wertuhowka', 'Jar', 'Bauobjekt 500', 'Slanzy', 'Nowosselje', 'Pljussa', 'Toroschino', 'Tschorskaja', 'Birlcino', 'Chruschtschowo', 'Sowchos Kuibyschew', 'Djagilewo', 'Grotowski', 'Iswest', 'Jambirino', 'Kurscha', 'Sowchos 15 Jahre Oktober', 'Sowchos Agronom', 'Sowchos Gorki', 'sarowka', 'Pronja', 'Kanischtschewskije Wysselki', 'Kriuscha', 'KanischtschewsIcije WysseLki', 'Murmino', 'Wyschgorod', 'Wyssokoje', 'Roshdestwenskoje', 'Roshdestwo', 'Schazk', 'StaroshiLowo', 'Zementja', 'Artjom', 'Bataisk', 'Bogdanowka', 'Bergwerkssiedlung SapadnoKapitalja', 'Bergwerkssiedlung ā€˛0GPU"', 'Gukowo', 'Gundorowski', 'Krasny Sulin', 'Nowikowka', 'RawnopoL', 'deshda', 'Mi ch aj Lo Leo n tj ewskaj a', 'Mich aj Lo Leo n tj e ws kaj a', 'Mi c h aj Lo Le o n tj e ws kaj a', 'MichajLoLeontjewskaja', 'chitschewanDonskaja', 'Nowotscherkassk', 'Nowoschachtinsk', 'MolotowSiedlung', 'Kamenolomni', 'Salsk', 'Schachtja', 'Schachty', 'Sinjawskaja', 'Taganrog', 'Tazinski', 'Arkadak', 'Podgorenka', 'Strecke Atkarsk...', 'Bagajewwka', 'Bagajewka', 'Balakowo', 'Chwalynsk', 'Engels', 'Grimm', 'Jekaterinowka', 'Jelschanka', 'Kologriwowka', 'Gorny', 'Kurdjum', 'Marx', 'Pudowkino', 'Pugatschow', 'Tamala', 'Toptulino', 'Trofimowski', 'Wertunowka', 'Wolsk', 'Dorogobush', 'Durowo', 'Gussino', 'Kolesniki', 'Kolesniki Swischtschewo', 'Kondrowo', 'Kuprino', 'Nikitinka', 'Pjatowskaja', 'Polotnjany Sawod', 'Priselskaja', 'Roslawl', 'Saretschje', 'Semlewo', 'Wjasma', 'Artscheda', 'Gorny Balyklej', 'Beketowka', 'Beketowskaja', 'Dubowka', 'Ilowlja', 'Kamyschin', 'Keller', 'Kotelnikowski', 'Sadowaja', 'Sarepta', 'Staraja Otrada', 'Urjupinsk', 'Wesjolaja Balka', 'Georgijewsk.', 'Neslobja', 'Jessentuki', 'Kislowodsk', 'Kumarinskie Schachty', 'Skatschki', 'Adui', 'Werchnjaja Sinjatschicha', 'Bobrowka', 'Mostyr', 'Altyi', 'Apparatja', 'Asanka', 'Asbest', 'Isumrud', 'Bashenowo', 'Belyje Baraki', 'Beresit', 'Berjosowski', 'Lossiny', 'Bogoslowsk', 'Chabartschicha', 'ChmyLowka', 'Chrompik', 'Chudjakowo', 'Degtjarsk', 'GLucharny', 'GorobLagodatskaja', 'Irbit', 'Kytlym', 'Isset', 'Istok', 'Iwdel2', 'Artjomowski', 'Krasnogwardejski', 'Jeshewaja', 'KamenskUralski', 'Kamyschlow', 'Karpinsk', 'Lewicha', 'Kolzowo', 'Kossulino', 'Kourowka', 'Krasnoturjinsk', 'Krasnotuijinsk', 'Koschai', 'Krasnouralsk', 'Krasny Alut', 'Krasny Jar', 'Krasny Shelesnjak', 'Maly Istok', 'Maramsino', 'Medja Schachta', 'Molwa', 'Monetny', 'Moroskowo', 'Mramorskaja', 'Mugai', 'deshdinsk', 'NejwoSchaitanski', 'WerchNejwinski', 'Basjanowski', 'Werchnije Sergi', 'Nishni Issetsk', 'Nishni Istok', 'Nishni Tagil', 'Nishnjaja Tura', 'Lobwa', 'Perschino', 'PerwouraLsk', 'PodwoLoschja', 'PokLjowskaja', 'Sewerski', 'Rasjesd 173', 'Rewda', 'Degtjarka', 'Sadanije', 'Samozwet', 'Schabrowski', 'Schaitan', 'Schartasch', 'Schuwakisch', 'Seredowi', 'Serow', 'Sewerouralsk', 'Sinjatschicha', 'Soswa', 'SredneuraLsk', 'Suchoi Log', 'BoLschoi Istok', 'TaLy Kljutsch', 'Tawda', 'TjopLy Kljutsch', 'Tschorja Retschka', 'Jertarski', 'Turin sk', 'Turinsk', 'Turinskie rudniki', 'Uktus', 'Urai', 'Werchnjaja Pyschma', 'Werchnjaja Saida', 'Otradnowo', 'Werchoturje', 'Wessjolowka', 'Woltschansk', 'Chobotowo', 'Inshawino', 'Kirsanow', 'Mitschurinsk', 'Morschansk', 'Oboro', 'Rada', 'Sampur', 'Wjashli', 'Kokschan', 'Bugulma', 'Buinsk', 'Jelabuga', 'Kamskoje Ustje', 'Kasan', 'Lubjany', 'Schemordan', 'SelenodoLsk', 'Tschistopol', 'Jalutorowsk', 'Lebedewka', 'WinsiLi', 'Myschega', 'BobrikDonskoi', 'Begitschewski', 'Suchodolski', 'Chomjakowo', 'Dedilowo', 'Sadonje', 'Smorodinka', 'Gorbatschowo', 'Granki', 'Jasja Polja', 'Wladimirskoje', 'Kasanowka', 'Lwowo', 'Maklez', 'Majenki', 'Obidimo', 'Obolenskoje', 'Plawsk', 'Polunino', 'Prisady', 'Sbrodowo', 'Schepeljowo', 'Ogarjowka', 'Serebrjanyje Prudy', 'Shdanka', 'Stalinogorsk', 'Tarusskaja', 'Towarkowo', 'Kossaja Gora', 'Bergwerkssiedlung Nr. 5', 'Torbejewka', 'Wenjow', 'Glasow', 'Ishewsk', 'Lynga', 'NjurdorKotja', 'Perwomaisk', 'Rjabowo', 'Wischur', 'Kondakowka', 'Poliwanowo', 'Taschla', 'Ai', 'Ascha', 'Bischkil', 'JemansheLinka', 'Korkino', 'Kamensk', 'Kamyschinka', 'Karabasch', 'Kartaly', 'Kopejsk', 'Kropatschowo', 'Magnitka', 'Kyschtym', 'Magnitogorsk', 'Mauk', 'Miass', 'Rosa', 'BakaL', 'Sawodskaja', 'Slatoust', 'Tajandy', 'Ufalej', 'Urshumka', 'Wawilowo', 'TschetschenAul', 'Samaschki', 'Gora Gorskaja', 'Grosny', 'AliJurt', 'sran', 'Basorkino', 'Scholkowskaja', 'TangiTschu', 'Tschita', 'Buguruslan', 'Koltubanka', 'Nowotroizk', 'Nowotroizki Maksai', 'Orsk', 'SoLILezk', 'ALatyr', 'Kasch', 'TjurLema', 'SawoLshskoje Torfopredprijatije', 'Tscheboksary', 'Butylizy', 'Anopino', 'Krasnoje Echo', 'Chrapowizkaja 1', 'Chrapowizkaja 2', 'Karabanowo', 'Koltschugino', 'Komissarowka', 'Iljitschowo', 'Melenki', 'Sarja', 'Sobinka', 'Andrejewo', 'Susdal', 'Tschulkowo', 'Undol', 'Mstjora', 'Woschod', 'Wtorowo', "Buschui'cha", 'Charowskaja', 'Dedowo Pole', 'Grjasowez', 'Jadricha', 'Jawenga', 'Kadnikow', 'Kirillow', 'Lesha', 'Petschatkino', 'Scheks', 'Schelomowo', 'Semigorodnjaja', 'Sokol', 'Suda', 'Ustjush', 'Wolonga', 'Woshega', 'Borissoglebsk', 'Chrenowaja', 'Ertil', 'Grafskaja', 'Gribanowka', 'Lipezk', 'Liski', 'Poworino', 'Rossosch', 'Usman', 'Tedshen', 'Jushny Alamyschik', 'Tschuama', 'Kokand', 'Skobelewo', 'Taschkent Tientjaksai', 'Achangaran', 'Angren', 'Angrenschachtstroi', 'Farchad', 'Jangijul', 'Gwardejski', 'Schirin', 'Charkow Ukraine', 'Anjewo', 'Perwuchino', 'Isjum', 'Krasnograd', 'Kupjansk', 'Ljubotin', 'Merefa', 'Nowaja Bawarija', 'Osnowa', 'Panjutino', 'Parchomowka', 'Pokotilowka', 'Rogan', 'Sokolniki', 'Tschugujew', 'Kriwin', 'Poninka', 'Schepetowka', 'Slawuta', 'Baglej', 'Baustelle des SamaraBergwerkes', 'Dneprodsershinsk', 'Dolginzewo', 'Kanderopol', 'Kriwoi Rog', 'Marganez', 'Nikopol', 'Nishnedneprowsk', 'Nowomoskowsk', 'Pereschtschepino', 'Prossjaja', 'Raduschja', 'Tschertomlyk', 'Tscherwonoje', 'Borislaw', 'Chodorow', 'Gelsendorf', 'Morschin', 'Shidatschow', 'Skole', 'Stryj', 'Tschernowizy', 'Belaja Zerkow', 'Borispol', 'Browary', 'Werchnjatschka', 'Darniza', 'Fastow', 'Sgurowka', 'Panfily', 'Schewtschenkowo', 'Schpola', 'Smela', 'Talnoje', 'Tscherkassy', 'Uman', 'Alexandria', 'Alexandrija', 'Kapitanowka', 'Wiska', 'Pantajewka', 'Sacharja', 'Scharowka', 'Schestakowka', 'Negrebki', 'Peremyschljany', 'Rudno', 'Salush', 'Sknilow', 'Skoschlow', 'SoLotschew', 'Berislaw', 'Cherson', 'Jurizino', 'Kopani', 'Partisany', 'SaLkowo', 'SokoLogornoje', 'Ternowka', 'Tschitschkarowka', 'NowoBorissow', 'Ismail', 'Kotowsk', 'Reni', 'Pestschanka', 'Abasowka', 'Chorol', 'Grebjonka', 'Oagotin', 'dagotin', 'Karlowka', 'Kononowka', 'Kotschubejewka', 'Krementschug', 'StaLinZuckerwerk', 'Lubny', 'Mechedowka', 'Pirjatin', 'Romodan', 'Skorochodowo', 'Wesjoly Podol', 'Sarny', 'Sdolbunow', 'Balabi no', 'Bolschoi Tokmak', 'Bolschoi Utljug', 'Georgijewski', 'Grigorjewka', 'Nowoalexejewka', 'Rosowka', 'Matwejewski', 'Melitopol', 'PetroMichajlowka', 'Mokraja', 'Nowogrigorjewka', 'Rodionowka', 'Terpenije', 'Belokorowitschi', 'Lyssaja Gora', 'Skomorochi', 'Chajtschnorin', 'Igtopol', 'Igtpot', 'Jemetjanowka', 'Korosten', 'Kriwoje', 'Matin', 'NowogradWolynski', 'Penisewitschi', 'Kornin', 'Radomyschl', 'Weledniki', 'Artjoma', 'Artjomowsk', 'Bairak', 'Chanshonkowo', 'Sujewka', 'Cholodja Balka', 'Debalzewo', 'Dobropolje', 'Dronowo', 'Drushkowka', 'FenoLja', 'GorLowka', 'Kondratjewski', 'Grechow', 'Jama', 'Jassinowka', 'Jekijewo', 'Kalmius', 'Katyk', 'Konstantinowka', 'Kramatorsk', 'Nowoekonomitscheskoje', 'Krasnoarmejskoe', 'Krasnogorowka', 'Kurachowka', 'Lutugino', 'Magdalinowka', 'Makejewka', 'Pantelejmonowka', 'Mospino', 'Motschalino', 'Motschalinski', 'Muschketowo', 'Nikitowka', 'Nowy Donbass', 'Orlowa Sloboda', 'Petrowka', 'Postnikowo', 'Roja', 'Rutschenkowo', 'Bergwerkssiedlung 12', 'Bergwerkssiedlung 31', 'Bergwerkssiedlung 56', 'Bergwerkssiedlung 78 BIS', 'Bergwerkssiedlung Dsershinka', 'Bergwerkssiedlung imeni Leni', 'Bergwerkssiedlung Kisseljow', 'Schtscheglowka', 'Shdanow', 'Skossyrskaja', 'Slawjansk', 'Smoljanka', 'Sol', 'Studgorodok', 'TroizkoCharzyssk', 'Trudowaja', 'Tschassow Jar', 'Tschistjakowo', 'Tschumakowo', 'Woskressenskaja', 'Zukuricha', 'Kolomyja', 'Lawotschnoje', 'Achtyrka', 'Gluchow', 'Konotop', 'Nepljujewo', 'Terny', 'Neptjujewo', 'PrawdinskiZuckerfabrik', 'Putiwt', 'Toropitowka', 'Torochtjany', 'Ternopot', 'Bobrowizy', 'T schernigow', 'Linowizy', 'Ladan', 'Perelowka', 'Safonowo Guta', 'SafonowoGuta', 'Semjonowka', 'Sorokatitschi', 'Tolsty Les', 'Wiltscha', 'Tschernjowka', 'Tschernowzy', 'GLuchowzy', 'Kasatin', 'Shmerinka', 'Gniwan', 'Manewitschi', 'Almasja', 'Altschewsk', 'Awdakowo', 'Bergwerk 2', 'Darjewka', 'Djakowo', 'Dolshanskaja', 'Golubowka', 'Gorskoje', 'Irmino', 'Iswarino', 'Kadijewka', 'Kamyschewacha', 'Karakas', 'KrasnopoLje', 'Krasny Lutsch', 'Krindatschowka', 'KrupskajaBergwerk', 'Lissitschansk', 'Kriworoshje', 'Manuilowka', 'Ovvragi', 'Parishskaja Kommu', 'Perejesdja', 'Popasja', 'Rowenki', 'Rubeshnoje', 'Schtschotowo', 'Schterowka', 'Sentjanowka', 'Sifonja', 'Simogorje', 'Waljanowski', 'Werchneje', 'Wiljanowo', 'Wolodarsk', 'ZentralnoBokowskoi', 'Chojniki', 'Dobrusch', 'Jelsk', 'Kostjukowka', 'Nowobelizkaja', 'Retschiza', 'Rogatschow', 'Shlobin', 'Sjabrowka', 'Usa', 'Wassilewitschi', 'Lida', 'Mosty', 'SLonim', 'PawLinowo', 'Ross', 'Borissow', 'Fanipol', 'Kolodischtschi', 'Krasnoje Smja', 'Maiji Gorka', 'Sedtscha', 'Shodino', 'SmoLewitschi', 'TaLka', 'Uretschje', 'Gluscha', 'Brosha', 'Bychow', 'Grodsjanka', 'Jasen', 'Kershenka', 'Kopzewitschi', 'Kritschew', 'Ossipowitschi', 'Mosyr', 'Barawucha', 'Tatarka', 'Schklow', 'Timkowitschi', 'Molodetschno', 'Oschmjany', 'Chljustino', 'Krasja', 'Obol', 'Osinowka', 'Ossipowka', 'Borkowitschi', 'Kriste', 'Tolotschin', 'Tschaschniki', 'Ulga', 'Koschtschu', 'Bernjaschen', 'Armenikend', 'Konju', 'Kukruse', 'Ereda', 'Aseri', 'Ellamaa', 'Jeschera', 'Agudseri', 'Gori', 'Borshomi', 'Inguri', 'SaganLugi', 'Sandary', 'MolotowStadtbezirk', 'Didube', 'wtlugi1 ā€¢', 'wibuli', 'Dshalombet', 'Giiterbahnhof', 'Ridder', 'Saschtschita', 'Kflkas', 'Sesava', 'Kalgi', 'Bukas', 'Spopane', 'Rembazkoje', 'Petrasiui', 'Seredzius', 'Lustberg', 'Airiogola', 'Klemiske', 'MaimaksanskiStadtbezirk', 'P ro leta rs ki Sta dtbezi rk', 'Stadtteil Beshiza', 'I wot', 'Kokino', 'Sakamensk', 'Nishnjaja Arakorka', 'Onochoj', 'AwtosawodskiStadtbezirk', 'SormowoStadtbezirk', 'SchumLewaja', 'Sejm a', 'Tschernoramenka', 'Gorschenino', 'not RSFSR as book says', 'Chlebowo', 'Krasny Orjol', 'Berendejewo', 'Karasch', 'Gusjatino', 'Migalewo', 'Grinino', 'Smoschje', 'Gontscharowo', 'Kotarowo', 'Tapiau', 'Jasnoje', 'Prochladja', 'Baltisk', 'Bagrationowsk', 'Neman', 'Sowetsk', 'Tramischen', 'Smensk', 'Sjassosero', 'Kappesalka', 'Kutichosskaja', 'Solomennoje', 'Kukowka', 'Witschka', 'Bessow Nos', 'Wolosniza', 'Pschada', 'Jejsk', 'Turgenewka', 'Subtschaninowka', 'Stary OskoL', 'Schemilowka', 'Ljamisa', 'Mstinski Most', 'Sowetski', 'Priosjorsk', 'Kin', 'Lissi Nos', 'Iwangorod', 'Kamenskoje', 'Seliwanowo', 'KowaLko', 'Dubowizkoje', 'Sosnowo', 'Selenogorsk', 'Kerest', 'UstSchora', 'UstSchory', 'Filippowka', 'Molebny Owrag', 'Stadtteil Rostokino', 'Werchnije Kotly', 'Nishnije Kotly', 'NowoGirejewo', 'Panki', 'Perowo Pole', 'Kurkino', 'Marfino', 'Alexino', 'Beskudnikowo', 'Guba', 'Nishnjaja Simba', 'Wladikawkas', 'Bugry', 'Kriwoschtschowoko', 'MBystraja', 'Dorogoi', 'Bolotowo', 'Kjutschiki', 'Raswilka', 'Jewlaschewo', 'Achuny', 'Machalino', 'Kripun', 'Rasjesd', 'Turns kaja', 'Turn a', 'Lesnoje', 'Sals k', 'NowoAsowka', 'Rostowka', 'Marzewo', 'Peschtschany', 'Sokolowaja Gora', 'Sudowka', 'km 282', 'DsershinskiStadtbezirk', 'JermanskiStadtbezirk', 'J e r m a n s ki Stad tbezi rk', 'TraktorosawodskiStadtbezirk', 'Sowchos Priwolshski', 'Shirnowsk', 'Abgenowo', 'Rasjesd Owrashny', 'Ajat', 'Turjinski', 'Kuschwa', 'Rudnitschny', 'Woltschanka', 'Bergwerk 23', 'Patotschja', 'Rogoshinski', 'Bergwerk 6', 'Stali nogorsk', 'Bergwerk 19', 'Dsjakino', 'Sareka', 'Moissejewka', 'Mjassnikowo', 'Nowoschalaschowo', 'Maksai', 'Korjakino', 'ski', 'Alexejewskoje', 'Makarji Roschtscha', 'Silno', 'Kokawino', 'Ljubomirskaja', 'Wassilkow', 'PostWolynski', 'Kowachino', 'Olesko', 'Sinilow', 'Rabotschi', 'Wetka', 'Kalinowka', 'PetrowoLidijewka', 'DonezkoAmwrossijewka', 'Shelanja', 'Schachtjorsk', 'Ilowaisk', 'Kriwoi Torez', 'Lidijewka', 'IwanoFrankowsk', 'Wiry', 'Jazewo', 'Shidinitschi', 'Uditschew', 'Uditschewo', 'Brjanka', 'DsershinskiBergwerk', 'Toschkowka', 'Maksimowka', 'Bergwerk Nr. 8', 'GLuboki', 'IljitschBergwerk', 'Petrowenki', 'Bergwerkssiedlung Nr. 7', 'Kosyrewo', 'Kasimirowka', 'Norio', 'Dshambejty', 'Komsomolski', 'Spass Saulok', 'B. Saborje', 'Werksiedlung Sulash Gora', 'Kudama', 'Repjowka', 'Sibrowo', 'Lopatkowo', 'Solnjetschja', 'Perewoloki', 'Alexejewka', 'Bachilowo', 'Uman', 'Owrutsch', 'Mariupol', 'Mokwa', 'Massy', 'Lahta', 'Sapjorja', 'Roshdestwennoje', 'Rybniza', 'Brno', 'Sibirien', 'Ural', 'Workuta', 'Magadan', 'Kolyma', 'Norilsk', 'Lubjanka', 'Butyrka', 'Kaliningrad', 'Lager Nr.', 'Lager Nummer']



```python
# Loading German language support from spaCy NLP library into a "language object" (nlp).
from spacy.lang.de import German
nlp = German()

# Tokenization of text in German language.
doc = nlp("Karl-Heinz Quade ist von März 1944 bis August 1948 im Lager 150 in Grjasowez interniert.")

# Loading Matcher support from spaCy NLP library.
from spacy.matcher import Matcher
matcher = Matcher(nlp.vocab)
```


```python
# Iterating through gazetteer list, using Matcher to establish, then match for, patterns.
for place in gazetteer:
    pattern = [{'LOWER': place.lower()}]
    matcher.add(place, [pattern])

# Use of Matcher tool to compose a list of patterns found in provided text, and their locations.
matches = matcher(doc)

# Printing starting and ending indices for patterns matched along tokens of the text.
for match_id, start, end in matches:
    print(start, end, doc[start:end].text)
```

    13 14 Grjasowez



```python
# Definition of pattern to be sought using Matcher.
pattern = [{'LOWER': 'larger'},
           {'LIKE_NUM': True}]

# Matching instances of tokens with "lager" (German for "camp"), when followed by a number, using Matcher.
matches.append([pattern])
```


```python
import requests
response = requests.get('https://programminghistorian.org/assets/finding-places-world-historical-gazetteer/place_texts.txt')
type(response)

response_text = response.text
type(response_text)

from bs4 import BeautifulSoup
soup = BeautifulSoup(response_text)
print(type(soup))
```

    <class 'bs4.BeautifulSoup'>



```python
# Application of Matcher functions to various .txt files in a directory, through iteration.
for file in Path('programming_historian').iterdir():
    doc = nlp(file.read_text(encoding='utf8'))
    matches = matcher(doc)
    for match_id, start, end in matches:
        print(file.name, start, end, doc[start:end].text)
```


    ---------------------------------------------------------------------------

    UnicodeDecodeError                        Traceback (most recent call last)

    Input In [42], in <cell line: 2>()
          1 # Application of Matcher functions to various .txt files in a directory, through iteration.
          2 for file in Path('programming_historian').iterdir():
    ----> 3     doc = nlp(file.read_text(encoding='utf8'))
          4     matches = matcher(doc)
          5     for match_id, start, end in matches:


    File ~/opt/anaconda3/lib/python3.9/pathlib.py:1267, in Path.read_text(self, encoding, errors)
       1263 """
       1264 Open the file in text mode, read it, and close the file.
       1265 """
       1266 with self.open(mode='r', encoding=encoding, errors=errors) as f:
    -> 1267     return f.read()


    File ~/opt/anaconda3/lib/python3.9/codecs.py:322, in BufferedIncrementalDecoder.decode(self, input, final)
        319 def decode(self, input, final=False):
        320     # decode input (taking the buffer into account)
        321     data = self.buffer + input
    --> 322     (result, consumed) = self._buffer_decode(data, self.errors, final)
        323     # keep undecoded input until the next call
        324     self.buffer = data[consumed:]


    UnicodeDecodeError: 'utf-8' codec can't decode byte 0x87 in position 23: invalid start byte



```python
# Loading Python's Counter tool.
from collections import Counter

# Addition of all matched terms to a list.
count_list = []
for match_id, start, end in matches:
    count_list.append(doc[start:end].text)
    
# Use of Counter tool to derive frequency of each term.
counter = Counter(count_list)

# Printing the ten most frequent matched terms.
for term, count in counter.most_common(10):
    print(term, count)
```

#### Application of Named Entity Recognition Tools.
* Important to assess results for validty, since they are machine-evaluated and prone to error.


```python
# Downloads "de_core_news_sm" pre-trained model.
!python -m spacy download de_core_news_sm

# Loads "de_core_news_sm" model for use in program into "language object" (nlp).
import spacy
nlp = spacy.load("de_core_news_sm")
```


```python
# Initialization of variable with model-processed text in German.
doc = nlp("Karl-Heinz Quade ist von März 1944 bis August 1948 im Lager 150 in Grjasowez interniert.")

# Printing of those locations identified by model in provided text.
for ent in doc.ents:
    print(ent.text, ent.label_, ent.start, ent.end)
```

#### Visualization of Index-Locations Pertaining to Matched Terms Along Text


```python
# Use of imported displaCy tool to visualize locations of matched terms along text.
from spacy import displacy
displacy.render(doc, jupyter = True, style = "ent")
```

#### Visualization of Relations Between Tokens Along Text


```python
# Use of displaCy tool to visualize relations between tokens along text.
displacy.render(doc, jupyter = True, style = "dep")
```


```python
from pathlib import Path

# Attempt at saving displacy visualizations to a file for use elsewhere.
svg = displacy.render(doc, style="dep")
output_path = Path("sentence.svg")
output_path.write_text(str(svg))
```


```python
# Installing DBpedia Spotlight library.
!pip install spacy-dbpedia-spotlight
```


```python
import spacy
nlp = spacy.load('de_core_news_sm')
nlp.add_pipe('dbpedia_spotlight', config = {'language_code': 'de'})

# Attempt at matching predicted entities with records to-be found in DBpedia.
doc = nlp("Karl-Heinz Quade ist von März 1944 bis August 1948 im Lager 150 in Grjasowez interniert.")
for ent in doc.ents:
    print(ent.text, ent.label_, ent.kb_id_)
```


```python
# Requesting access to data associated with a match for "Grjasowez" from the DBpedia library.
import requests
data = requests.get("http://de.dbpedia.org/data/Grjasowez.json").json()
```


```python
# Accessing contents of data associated with a match for "Grjasowez" from the DBpedia library.
data
```


```python
# Accessing contents of those keys utilized in data dictionary associated with a match for "Grjasowez" from the DBpedia library.
data.keys()
```


```python
# Accessing latitude and longitutde of Grjasowez using DBpedia.
print(f"Latitude and Longitude of Grjasowez: {data['http://de.dbpedia.org/resource/Grjasowez']['http://www.georss.org/georss/point'][0]['value']}")
```


```python
# Processing data into a TSV format file suitable for exporting processes.
start_date = "1800"
end_date = "2000"
source_title = "Karl-Heinz Quade Diary"

output_text = ""
column_header = "id\ttitle\ttitle_source\tstart\tend\n"
output_text += column_header

places_list = []
if matches:
    places_list.extend([doc[start:end].text for match_id, start, end in matches])
if doc.ents:
    places_list.extend([ent.text for ent in doc.ents if ent.label_ == "GPE" or ent.label_ == "LOC"])

unique_places = set(places_list)

for id, place in enumerate(unique_places):
    output_text += f'{id}\t{place}\t{source_title}\t{start_date}\t{end_date}\n'
    
filename = source_title.lower().replace(' ', '_') + '.tsv'
Path(filename).write_text(output_text)
print('created: ', filename)
```

### Reflection
This Programming Historian lesson introduced me to some of the methods used by Digital Historians to scan series of text for terms of interest, with a particular focus on those terms associated with geographical locations. Gazeteers serve as directories containing a series of location names. Using such gazetteers as references, we are able to associate words from text as terms pertaining to the names of locations. This lesson introduced various gazetteers, however, the World Historical Gazetteer remains of special interest to me. This gazetteer serves as a database that has accumulated the names of geographical locations as they have changed throughout history. Using this database, digital historians have been offered the flexibility to analyze texts of historical periods at even greater degrees. Further, this lesson introduced me to Natural Language Processing tools that allow digital historians to analyze texts that have experienced transliteration, that is, the process of having been translated across languages and alphabets.

I see a great deal of potential within the computational methods this lesson introduced me to. Already, the World Historical Gazetteer contains an incredible amount of information about geographical regions across vast time periods. I feel excited to see all this great resource may offer in the future as researchers continue to add to this repository. In terms of what this resource may offer to humanist research, I believe that it will be of great aid in specifying the geographic and historical contexts for a piece of humanist evidence, which would surely offer provide researchers with a greater degree of freedom and accuracy when aiming to interpret such evidence in relation to the subject's sociocultural and historical contexts.
