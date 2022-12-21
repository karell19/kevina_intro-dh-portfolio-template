---
layout: page
title: Finding Places in Text with the World Historical Gazeteer
description: This Programming Historian lesson has taught me about the resources available to digital historians that allow them to learn about geographical locations mentioned throughout texts, and further, to visualize such locations for reconciliation and geocoding purposes. 
---
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
    Requirement already satisfied: spacy-legacy<3.1.0,>=3.0.9 in /Users/kevin/opt/anaconda3/lib/python3.9/site-packages (from spacy) (3.0.10)
    Requirement already satisfied: spacy-loggers<2.0.0,>=1.0.0 in /Users/kevin/opt/anaconda3/lib/python3.9/site-packages (from spacy) (1.0.3)
    Requirement already satisfied: packaging>=20.0 in /Users/kevin/opt/anaconda3/lib/python3.9/site-packages (from spacy) (21.3)
    Requirement already satisfied: thinc<8.2.0,>=8.1.0 in /Users/kevin/opt/anaconda3/lib/python3.9/site-packages (from spacy) (8.1.3)
    Requirement already satisfied: typer<0.5.0,>=0.3.0 in /Users/kevin/opt/anaconda3/lib/python3.9/site-packages (from spacy) (0.4.2)
    Requirement already satisfied: langcodes<4.0.0,>=3.2.0 in /Users/kevin/opt/anaconda3/lib/python3.9/site-packages (from spacy) (3.3.0)
    Requirement already satisfied: srsly<3.0.0,>=2.4.3 in /Users/kevin/opt/anaconda3/lib/python3.9/site-packages (from spacy) (2.4.4)
    Requirement already satisfied: cymem<2.1.0,>=2.0.2 in /Users/kevin/opt/anaconda3/lib/python3.9/site-packages (from spacy) (2.0.6)
    Requirement already satisfied: requests<3.0.0,>=2.13.0 in /Users/kevin/opt/anaconda3/lib/python3.9/site-packages (from spacy) (2.27.1)
    Requirement already satisfied: numpy>=1.15.0 in /Users/kevin/opt/anaconda3/lib/python3.9/site-packages (from spacy) (1.21.5)
    Requirement already satisfied: setuptools in /Users/kevin/opt/anaconda3/lib/python3.9/site-packages (from spacy) (61.2.0)
    Requirement already satisfied: pathy>=0.3.5 in /Users/kevin/opt/anaconda3/lib/python3.9/site-packages (from spacy) (0.6.2)
    Requirement already satisfied: jinja2 in /Users/kevin/opt/anaconda3/lib/python3.9/site-packages (from spacy) (2.11.3)
    Requirement already satisfied: wasabi<1.1.0,>=0.9.1 in /Users/kevin/opt/anaconda3/lib/python3.9/site-packages (from spacy) (0.10.1)
    Requirement already satisfied: catalogue<2.1.0,>=2.0.6 in /Users/kevin/opt/anaconda3/lib/python3.9/site-packages (from spacy) (2.0.8)
    Requirement already satisfied: tqdm<5.0.0,>=4.38.0 in /Users/kevin/opt/anaconda3/lib/python3.9/site-packages (from spacy) (4.64.0)
    Requirement already satisfied: pydantic!=1.8,!=1.8.1,<1.10.0,>=1.7.4 in /Users/kevin/opt/anaconda3/lib/python3.9/site-packages (from spacy) (1.9.2)
    Requirement already satisfied: murmurhash<1.1.0,>=0.28.0 in /Users/kevin/opt/anaconda3/lib/python3.9/site-packages (from spacy) (1.0.8)
    Requirement already satisfied: preshed<3.1.0,>=3.0.2 in /Users/kevin/opt/anaconda3/lib/python3.9/site-packages (from spacy) (3.0.7)
    Requirement already satisfied: pyparsing!=3.0.5,>=2.0.2 in /Users/kevin/opt/anaconda3/lib/python3.9/site-packages (from packaging>=20.0->spacy) (3.0.4)
    Requirement already satisfied: smart-open<6.0.0,>=5.2.1 in /Users/kevin/opt/anaconda3/lib/python3.9/site-packages (from pathy>=0.3.5->spacy) (5.2.1)
    Requirement already satisfied: typing-extensions>=3.7.4.3 in /Users/kevin/opt/anaconda3/lib/python3.9/site-packages (from pydantic!=1.8,!=1.8.1,<1.10.0,>=1.7.4->spacy) (4.1.1)
    Requirement already satisfied: idna<4,>=2.5 in /Users/kevin/opt/anaconda3/lib/python3.9/site-packages (from requests<3.0.0,>=2.13.0->spacy) (3.3)
    Requirement already satisfied: charset-normalizer~=2.0.0 in /Users/kevin/opt/anaconda3/lib/python3.9/site-packages (from requests<3.0.0,>=2.13.0->spacy) (2.0.4)
    Requirement already satisfied: certifi>=2017.4.17 in /Users/kevin/opt/anaconda3/lib/python3.9/site-packages (from requests<3.0.0,>=2.13.0->spacy) (2021.10.8)
    Requirement already satisfied: urllib3<1.27,>=1.21.1 in /Users/kevin/opt/anaconda3/lib/python3.9/site-packages (from requests<3.0.0,>=2.13.0->spacy) (1.26.9)
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
# Application of Matcher functions to various .txt files in a directory, through iteration.
with open('programming_historian_copy/place_texts.txt', 'r') as f:
    doc = nlp(f.read())
    matches = matcher(doc)
    for match_id, start, end in matches:
        print(f.name, start, end, doc[start:end].text)
```

    programming_historian_copy/place_texts.txt 16 17 Stalingrad
    programming_historian_copy/place_texts.txt 33 34 Workuta
    programming_historian_copy/place_texts.txt 35 36 Astrachan
    programming_historian_copy/place_texts.txt 65 66 Stalingrad
    programming_historian_copy/place_texts.txt 77 78 Stalingrad
    programming_historian_copy/place_texts.txt 104 105 Stalingrad
    programming_historian_copy/place_texts.txt 106 107 Saratow
    programming_historian_copy/place_texts.txt 180 181 Stalingrad
    programming_historian_copy/place_texts.txt 190 191 Jelabuga
    programming_historian_copy/place_texts.txt 193 194 Stalingrad
    programming_historian_copy/place_texts.txt 222 223 Stalingrad
    programming_historian_copy/place_texts.txt 240 241 Jelabuga
    programming_historian_copy/place_texts.txt 382 383 Stalingrad
    programming_historian_copy/place_texts.txt 396 397 Stalingrad
    programming_historian_copy/place_texts.txt 416 417 Stalingrad
    programming_historian_copy/place_texts.txt 456 457 Stalingrad
    programming_historian_copy/place_texts.txt 483 484 Stalingrad
    programming_historian_copy/place_texts.txt 521 522 Stalingrad
    programming_historian_copy/place_texts.txt 600 601 Moskau
    programming_historian_copy/place_texts.txt 649 650 Stalingrad
    programming_historian_copy/place_texts.txt 663 664 Stalingrad
    programming_historian_copy/place_texts.txt 715 716 Stalingrad
    programming_historian_copy/place_texts.txt 730 731 Stalingrad
    programming_historian_copy/place_texts.txt 794 795 Stalingrad
    programming_historian_copy/place_texts.txt 825 826 Russland
    programming_historian_copy/place_texts.txt 861 862 Stalingrad
    programming_historian_copy/place_texts.txt 880 881 Stalingrad
    programming_historian_copy/place_texts.txt 971 972 Stalingrad
    programming_historian_copy/place_texts.txt 1056 1057 Stalingrad
    programming_historian_copy/place_texts.txt 1081 1082 Stalingrad
    programming_historian_copy/place_texts.txt 1119 1120 Stalingrad
    programming_historian_copy/place_texts.txt 1140 1141 Stalingrad
    programming_historian_copy/place_texts.txt 1166 1167 Stalingrad
    programming_historian_copy/place_texts.txt 1186 1187 Stalingrad
    programming_historian_copy/place_texts.txt 1241 1242 Stalingrad
    programming_historian_copy/place_texts.txt 1275 1276 Stalingrad
    programming_historian_copy/place_texts.txt 1336 1337 Stalingrad
    programming_historian_copy/place_texts.txt 1456 1457 Stalingrad
    programming_historian_copy/place_texts.txt 1483 1484 Russland
    programming_historian_copy/place_texts.txt 1506 1507 Stalingrad
    programming_historian_copy/place_texts.txt 1563 1564 Stalingrad
    programming_historian_copy/place_texts.txt 1631 1632 Stalingrad
    programming_historian_copy/place_texts.txt 1682 1683 Stalingrad
    programming_historian_copy/place_texts.txt 1744 1745 Stalingrad
    programming_historian_copy/place_texts.txt 1769 1770 Russland
    programming_historian_copy/place_texts.txt 1795 1796 Stalingrad
    programming_historian_copy/place_texts.txt 1830 1831 Stalingrad
    programming_historian_copy/place_texts.txt 1919 1920 Stalingrad
    programming_historian_copy/place_texts.txt 1931 1932 Stalingrad
    programming_historian_copy/place_texts.txt 1940 1941 Russland
    programming_historian_copy/place_texts.txt 1949 1950 Stalingrad
    programming_historian_copy/place_texts.txt 1977 1978 Stalingrad
    programming_historian_copy/place_texts.txt 1985 1986 Stalingrad
    programming_historian_copy/place_texts.txt 2133 2134 Stalingrad
    programming_historian_copy/place_texts.txt 2364 2365 Keller
    programming_historian_copy/place_texts.txt 2484 2485 Stalingrad
    programming_historian_copy/place_texts.txt 2516 2517 Stalingrad
    programming_historian_copy/place_texts.txt 2558 2559 Gorki
    programming_historian_copy/place_texts.txt 2570 2571 Moskau
    programming_historian_copy/place_texts.txt 2592 2593 Stalingrad
    programming_historian_copy/place_texts.txt 2594 2595 Saratow
    programming_historian_copy/place_texts.txt 2596 2597 Wolsk
    programming_historian_copy/place_texts.txt 2598 2599 Sysran
    programming_historian_copy/place_texts.txt 2600 2601 Kuibyschew
    programming_historian_copy/place_texts.txt 2604 2605 Uljanowsk
    programming_historian_copy/place_texts.txt 2606 2607 Kasan
    programming_historian_copy/place_texts.txt 2610 2611 Kasan
    programming_historian_copy/place_texts.txt 2612 2613 Moskau
    programming_historian_copy/place_texts.txt 2714 2715 Kasan
    programming_historian_copy/place_texts.txt 2765 2766 Stalingrad
    programming_historian_copy/place_texts.txt 2834 2835 Stalingrad
    programming_historian_copy/place_texts.txt 3002 3003 Kasan
    programming_historian_copy/place_texts.txt 3039 3040 Kasan
    programming_historian_copy/place_texts.txt 3079 3080 Kasan
    programming_historian_copy/place_texts.txt 3113 3114 Tscheboksary
    programming_historian_copy/place_texts.txt 3139 3140 Tula
    programming_historian_copy/place_texts.txt 3193 3194 Russland
    programming_historian_copy/place_texts.txt 3259 3260 Moskau
    programming_historian_copy/place_texts.txt 3267 3268 Jelabuga
    programming_historian_copy/place_texts.txt 3520 3521 Kaluga
    programming_historian_copy/place_texts.txt 3529 3530 Tula
    programming_historian_copy/place_texts.txt 3553 3554 Moskau
    programming_historian_copy/place_texts.txt 3570 3571 Derbent
    programming_historian_copy/place_texts.txt 3574 3575 Baku
    programming_historian_copy/place_texts.txt 3587 3588 Astrachan
    programming_historian_copy/place_texts.txt 3592 3593 Moskau
    programming_historian_copy/place_texts.txt 3608 3609 Pugatschow
    programming_historian_copy/place_texts.txt 3626 3627 Moskau
    programming_historian_copy/place_texts.txt 3714 3715 Ural
    programming_historian_copy/place_texts.txt 3721 3722 Ural
    programming_historian_copy/place_texts.txt 3730 3731 Kasan
    programming_historian_copy/place_texts.txt 3733 3734 Pugatschow
    programming_historian_copy/place_texts.txt 3738 3739 Moskau
    programming_historian_copy/place_texts.txt 4066 4067 Moskau
    programming_historian_copy/place_texts.txt 4121 4122 Smolensk
    programming_historian_copy/place_texts.txt 4139 4140 Smolensk
    programming_historian_copy/place_texts.txt 4237 4238 Workuta
    programming_historian_copy/place_texts.txt 4239 4240 Astrachan
    programming_historian_copy/place_texts.txt 4311 4312 Smolensk
    programming_historian_copy/place_texts.txt 4323 4324 Sibirien
    programming_historian_copy/place_texts.txt 4466 4467 Smolensk
    programming_historian_copy/place_texts.txt 4494 4495 Smolensk
    programming_historian_copy/place_texts.txt 4568 4569 Smolensk
    programming_historian_copy/place_texts.txt 4596 4597 Johannes
    programming_historian_copy/place_texts.txt 4604 4605 Smolensk
    programming_historian_copy/place_texts.txt 4627 4628 Smolensk
    programming_historian_copy/place_texts.txt 5097 5098 Borowitschi
    programming_historian_copy/place_texts.txt 5121 5122 KALININ
    programming_historian_copy/place_texts.txt 5127 5128 Borissow
    programming_historian_copy/place_texts.txt 5130 5131 MINSK
    programming_historian_copy/place_texts.txt 5132 5133 Orscha
    programming_historian_copy/place_texts.txt 5138 5139 WITEBSK
    programming_historian_copy/place_texts.txt 5156 5157 LWOW
    programming_historian_copy/place_texts.txt 5167 5168 Stanislaw
    programming_historian_copy/place_texts.txt 5180 5181 Winniza
    programming_historian_copy/place_texts.txt 5182 5183 Tscherkassy
    programming_historian_copy/place_texts.txt 5184 5185 Molotowsk
    programming_historian_copy/place_texts.txt 5193 5194 SMOLENSK
    programming_historian_copy/place_texts.txt 5196 5197 MOSKAU
    programming_historian_copy/place_texts.txt 5200 5201 KALUGA
    programming_historian_copy/place_texts.txt 5217 5218 BRJANSK
    programming_historian_copy/place_texts.txt 5221 5222 Lebedjan
    programming_historian_copy/place_texts.txt 5224 5225 ARCHANGELSK
    programming_historian_copy/place_texts.txt 5227 5228 Temnikow
    programming_historian_copy/place_texts.txt 5230 5231 PENSA
    programming_historian_copy/place_texts.txt 5232 5233 Wolsk
    programming_historian_copy/place_texts.txt 5248 5249 Kineschma
    programming_historian_copy/place_texts.txt 5257 5258 Susdal
    programming_historian_copy/place_texts.txt 5263 5264 WLADIMIR
    programming_historian_copy/place_texts.txt 5294 5295 KURSK
    programming_historian_copy/place_texts.txt 5297 5298 Sumy
    programming_historian_copy/place_texts.txt 5312 5313 ODESSA
    programming_historian_copy/place_texts.txt 5319 5320 POLTAWA
    programming_historian_copy/place_texts.txt 5320 5321 CHARKOW
    programming_historian_copy/place_texts.txt 5325 5326 Kupjansk
    programming_historian_copy/place_texts.txt 5329 5330 Atkarsk
    programming_historian_copy/place_texts.txt 5331 5332 SARATOW
    programming_historian_copy/place_texts.txt 5333 5334 Urjupinsk
    programming_historian_copy/place_texts.txt 5335 5336 Lissitschansk
    programming_historian_copy/place_texts.txt 5341 5342 Rjasan
    programming_historian_copy/place_texts.txt 5346 5347 Potma
    programming_historian_copy/place_texts.txt 5348 5349 Saransk
    programming_historian_copy/place_texts.txt 5351 5352 Morschansk
    programming_historian_copy/place_texts.txt 5355 5356 Kramatorsk
    programming_historian_copy/place_texts.txt 5360 5361 MAKEJEWKA
    programming_historian_copy/place_texts.txt 5364 5365 STALINO
    programming_historian_copy/place_texts.txt 5370 5371 Melitopol
    programming_historian_copy/place_texts.txt 5373 5374 Taganrog
    programming_historian_copy/place_texts.txt 5381 5382 SIMFEROPOL
    programming_historian_copy/place_texts.txt 5394 5395 Armawir
    programming_historian_copy/place_texts.txt 5401 5402 Nowotscherkassk
    programming_historian_copy/place_texts.txt 5442 5443 Rustawi
    programming_historian_copy/place_texts.txt 5451 5452 TASCHKENT
    programming_historian_copy/place_texts.txt 5453 5454 Tschuama
    programming_historian_copy/place_texts.txt 5459 5460 Kokand
    programming_historian_copy/place_texts.txt 5467 5468 Begowat
    programming_historian_copy/place_texts.txt 5717 5718 Kertsch
    programming_historian_copy/place_texts.txt 5771 5772 Maikop
    programming_historian_copy/place_texts.txt 5878 5879 Baku
    programming_historian_copy/place_texts.txt 5944 5945 Stalingrad
    programming_historian_copy/place_texts.txt 5991 5992 Astrachan
    programming_historian_copy/place_texts.txt 6057 6058 Tichorezk
    programming_historian_copy/place_texts.txt 6082 6083 Stalingrad
    programming_historian_copy/place_texts.txt 6087 6088 Astrachan
    programming_historian_copy/place_texts.txt 6100 6101 Stalingrad
    programming_historian_copy/place_texts.txt 6108 6109 Astrachan
    programming_historian_copy/place_texts.txt 6188 6189 Baku
    programming_historian_copy/place_texts.txt 6297 6298 Kertsch
    programming_historian_copy/place_texts.txt 6343 6344 Kertsch
    programming_historian_copy/place_texts.txt 6440 6441 Leningrad
    programming_historian_copy/place_texts.txt 6497 6498 Krasnogorsk
    programming_historian_copy/place_texts.txt 6499 6500 Moskau
    programming_historian_copy/place_texts.txt 6622 6623 Moskau
    programming_historian_copy/place_texts.txt 6653 6654 Moskau
    programming_historian_copy/place_texts.txt 6671 6672 Moskau
    programming_historian_copy/place_texts.txt 6826 6827 Jelabuga
    programming_historian_copy/place_texts.txt 6884 6885 Engels
    programming_historian_copy/place_texts.txt 6951 6952 Engels
    programming_historian_copy/place_texts.txt 7051 7052 Moskau
    programming_historian_copy/place_texts.txt 7064 7065 Moskau
    programming_historian_copy/place_texts.txt 7074 7075 Moskau
    programming_historian_copy/place_texts.txt 7139 7140 Moskau
    programming_historian_copy/place_texts.txt 7154 7155 Moskau
    programming_historian_copy/place_texts.txt 7177 7178 Krasnogorsk
    programming_historian_copy/place_texts.txt 7201 7202 Moskau
    programming_historian_copy/place_texts.txt 7309 7310 Marx
    programming_historian_copy/place_texts.txt 7364 7365 Gorki
    programming_historian_copy/place_texts.txt 7425 7426 Nowgorod
    programming_historian_copy/place_texts.txt 7428 7429 Gorki
    programming_historian_copy/place_texts.txt 7448 7449 Wladimir
    programming_historian_copy/place_texts.txt 7472 7473 Kasan
    programming_historian_copy/place_texts.txt 7497 7498 Nowgorod
    programming_historian_copy/place_texts.txt 7525 7526 Nowgorod
    programming_historian_copy/place_texts.txt 7557 7558 Moskau
    programming_historian_copy/place_texts.txt 7620 7621 Susdal
    programming_historian_copy/place_texts.txt 7631 7632 Moskau
    programming_historian_copy/place_texts.txt 7638 7639 Jelabuga
    programming_historian_copy/place_texts.txt 7719 7720 Gorki
    programming_historian_copy/place_texts.txt 7722 7723 Moskau
    programming_historian_copy/place_texts.txt 7724 7725 Leningrad
    programming_historian_copy/place_texts.txt 7732 7733 RSFSR
    programming_historian_copy/place_texts.txt 7901 7902 Gorki
    programming_historian_copy/place_texts.txt 7949 7950 Nowgorod
    programming_historian_copy/place_texts.txt 7955 7956 Gorki
    programming_historian_copy/place_texts.txt 7985 7986 Gorki
    programming_historian_copy/place_texts.txt 7988 7989 Nowgorod
    programming_historian_copy/place_texts.txt 8044 8045 Nowgorod
    programming_historian_copy/place_texts.txt 8050 8051 Moskau
    programming_historian_copy/place_texts.txt 8093 8094 Gorki
    programming_historian_copy/place_texts.txt 8235 8236 Jelabuga
    programming_historian_copy/place_texts.txt 8468 8469 Jelabuga
    programming_historian_copy/place_texts.txt 8484 8485 Jelabuga
    programming_historian_copy/place_texts.txt 8518 8519 Jelabuga
    programming_historian_copy/place_texts.txt 8535 8536 Jelabuga
    programming_historian_copy/place_texts.txt 8561 8562 Selenodolsk
    programming_historian_copy/place_texts.txt 8567 8568 Kasan
    programming_historian_copy/place_texts.txt 8573 8574 Selenodolsk
    programming_historian_copy/place_texts.txt 8580 8581 Selenodolsk
    programming_historian_copy/place_texts.txt 8587 8588 Selenodolsk
    programming_historian_copy/place_texts.txt 8602 8603 Moskau
    programming_historian_copy/place_texts.txt 8604 8605 Jarzewo
    programming_historian_copy/place_texts.txt 8610 8611 Jarzewo
    programming_historian_copy/place_texts.txt 8623 8624 Charkow
    programming_historian_copy/place_texts.txt 8635 8636 Charkow
    programming_historian_copy/place_texts.txt 8637 8638 Ukraine
    programming_historian_copy/place_texts.txt 9831 9832 Ukraine
    programming_historian_copy/place_texts.txt 9841 9842 Litauen
    programming_historian_copy/place_texts.txt 9843 9844 Estland
    programming_historian_copy/place_texts.txt 9980 9981 Wladimir
    programming_historian_copy/place_texts.txt 10114 10115 Moskau
    programming_historian_copy/place_texts.txt 10129 10130 Leningrad
    programming_historian_copy/place_texts.txt 10319 10320 Ural
    programming_historian_copy/place_texts.txt 10325 10326 Sibirien
    programming_historian_copy/place_texts.txt 10404 10405 Stalingrad
    programming_historian_copy/place_texts.txt 10590 10591 RSFSR
    programming_historian_copy/place_texts.txt 10597 10598 Ukraine
    programming_historian_copy/place_texts.txt 10601 10602 Usbekistan
    programming_historian_copy/place_texts.txt 10603 10604 Kasachstan
    programming_historian_copy/place_texts.txt 10605 10606 Georgien
    programming_historian_copy/place_texts.txt 10609 10610 Litauen
    programming_historian_copy/place_texts.txt 10611 10612 Estland
    programming_historian_copy/place_texts.txt 10617 10618 Armenien
    programming_historian_copy/place_texts.txt 10623 10624 Lettland
    programming_historian_copy/place_texts.txt 10791 10792 Kyschtym
    programming_historian_copy/place_texts.txt 10795 10796 Tscheljabinsk
    programming_historian_copy/place_texts.txt 10803 10804 Beketowka
    programming_historian_copy/place_texts.txt 10834 10835 Orscha
    programming_historian_copy/place_texts.txt 10836 10837 Beketowka
    programming_historian_copy/place_texts.txt 10838 10839 Wolsk
    programming_historian_copy/place_texts.txt 10865 10866 Kasan
    programming_historian_copy/place_texts.txt 10867 10868 Stalingrad
    programming_historian_copy/place_texts.txt 10878 10879 Workuta
    programming_historian_copy/place_texts.txt 10893 10894 Moskau
    programming_historian_copy/place_texts.txt 10895 10896 Workuta
    programming_historian_copy/place_texts.txt 10948 10949 Riga
    programming_historian_copy/place_texts.txt 10969 10970 Suchumi
    programming_historian_copy/place_texts.txt 10981 10982 Nowoschachtinsk
    programming_historian_copy/place_texts.txt 10993 10994 Brjansk
    programming_historian_copy/place_texts.txt 11009 11010 Stalingrad
    programming_historian_copy/place_texts.txt 11045 11046 Ragnit
    programming_historian_copy/place_texts.txt 11047 11048 Sibirien
    programming_historian_copy/place_texts.txt 11066 11067 Orsk
    programming_historian_copy/place_texts.txt 11085 11086 Moskau
    programming_historian_copy/place_texts.txt 11087 11088 Krasnogorsk
    programming_historian_copy/place_texts.txt 11390 11391 Moskau
    programming_historian_copy/place_texts.txt 11505 11506 Moskau
    programming_historian_copy/place_texts.txt 11519 11520 Moskau
    programming_historian_copy/place_texts.txt 11598 11599 Moskau
    programming_historian_copy/place_texts.txt 11617 11618 Moskau
    programming_historian_copy/place_texts.txt 11642 11643 Moskau
    programming_historian_copy/place_texts.txt 11659 11660 Krjukowo
    programming_historian_copy/place_texts.txt 11661 11662 Kaschira
    programming_historian_copy/place_texts.txt 11671 11672 Moskau
    programming_historian_copy/place_texts.txt 11690 11691 Moskau
    programming_historian_copy/place_texts.txt 11753 11754 Moskau
    programming_historian_copy/place_texts.txt 11963 11964 Moskau
    programming_historian_copy/place_texts.txt 11976 11977 Ural
    programming_historian_copy/place_texts.txt 11981 11982 Moskau
    programming_historian_copy/place_texts.txt 12049 12050 Moskau
    programming_historian_copy/place_texts.txt 12367 12368 Ar
    programming_historian_copy/place_texts.txt 12400 12401 Stalingrad
    programming_historian_copy/place_texts.txt 12431 12432 Stalingrad
    programming_historian_copy/place_texts.txt 12514 12515 Keller
    programming_historian_copy/place_texts.txt 12632 12633 Stalingrad
    programming_historian_copy/place_texts.txt 12645 12646 Stalingrad
    programming_historian_copy/place_texts.txt 12787 12788 Stalingrad
    programming_historian_copy/place_texts.txt 12867 12868 Russland
    programming_historian_copy/place_texts.txt 13011 13012 STALINGRAD
    programming_historian_copy/place_texts.txt 13055 13056 Stalingrad
    programming_historian_copy/place_texts.txt 13288 13289 Nowotroizk
    programming_historian_copy/place_texts.txt 13290 13291 Maksai
    programming_historian_copy/place_texts.txt 13608 13609 Moskau
    programming_historian_copy/place_texts.txt 13694 13695 Stalingrad
    programming_historian_copy/place_texts.txt 13715 13716 Rshew
    programming_historian_copy/place_texts.txt 13748 13749 Bor
    programming_historian_copy/place_texts.txt 13754 13755 Smolensk
    programming_historian_copy/place_texts.txt 13767 13768 Rshew
    programming_historian_copy/place_texts.txt 13772 13773 Gshatsk
    programming_historian_copy/place_texts.txt 13783 13784 Brjansk
    programming_historian_copy/place_texts.txt 13785 13786 Orjol
    programming_historian_copy/place_texts.txt 13793 13794 Smolensk
    programming_historian_copy/place_texts.txt 13821 13822 Witebsk
    programming_historian_copy/place_texts.txt 13833 13834 Roslawl
    programming_historian_copy/place_texts.txt 13845 13846 Brjansk
    programming_historian_copy/place_texts.txt 13852 13853 Smolensk
    programming_historian_copy/place_texts.txt 13875 13876 Minsk
    programming_historian_copy/place_texts.txt 13963 13964 Jelabuga
    programming_historian_copy/place_texts.txt 13995 13996 Kasan
    programming_historian_copy/place_texts.txt 14013 14014 Selenodolsk
    programming_historian_copy/place_texts.txt 14036 14037 Jelabuga
    programming_historian_copy/place_texts.txt 14078 14079 Kasan
    programming_historian_copy/place_texts.txt 14093 14094 Kasan
    programming_historian_copy/place_texts.txt 14111 14112 Selenodolsk
    programming_historian_copy/place_texts.txt 14123 14124 Selenodolsk
    programming_historian_copy/place_texts.txt 14148 14149 Kasan
    programming_historian_copy/place_texts.txt 14197 14198 Kasan
    programming_historian_copy/place_texts.txt 14231 14232 Kasan
    programming_historian_copy/place_texts.txt 14246 14247 Kasan
    programming_historian_copy/place_texts.txt 14267 14268 Kasan
    programming_historian_copy/place_texts.txt 14321 14322 Kasan
    programming_historian_copy/place_texts.txt 14370 14371 Selenodolsk
    programming_historian_copy/place_texts.txt 14533 14534 Selenodolsk
    programming_historian_copy/place_texts.txt 14607 14608 Selenodolsk
    programming_historian_copy/place_texts.txt 14654 14655 Selenodolsk
    programming_historian_copy/place_texts.txt 14721 14722 Riga
    programming_historian_copy/place_texts.txt 14736 14737 Selenodolsk
    programming_historian_copy/place_texts.txt 14769 14770 Selenodolsk
    programming_historian_copy/place_texts.txt 14846 14847 Selenodolsk
    programming_historian_copy/place_texts.txt 14944 14945 Tiraspol
    programming_historian_copy/place_texts.txt 14994 14995 Nikolajew
    programming_historian_copy/place_texts.txt 15108 15109 Odessa
    programming_historian_copy/place_texts.txt 15149 15150 Nikolajew
    programming_historian_copy/place_texts.txt 15195 15196 Nikolajew
    programming_historian_copy/place_texts.txt 15237 15238 Nikolajew
    programming_historian_copy/place_texts.txt 15311 15312 Nikolajew
    programming_historian_copy/place_texts.txt 15419 15420 Odessa
    programming_historian_copy/place_texts.txt 15437 15438 Odessa
    programming_historian_copy/place_texts.txt 15490 15491 Nikolajew
    programming_historian_copy/place_texts.txt 15498 15499 Odessa
    programming_historian_copy/place_texts.txt 15506 15507 Odessa
    programming_historian_copy/place_texts.txt 15523 15524 Odessa
    programming_historian_copy/place_texts.txt 15534 15535 Krim
    programming_historian_copy/place_texts.txt 15547 15548 Odessa
    programming_historian_copy/place_texts.txt 15613 15614 Odessa
    programming_historian_copy/place_texts.txt 15618 15619 Krim
    programming_historian_copy/place_texts.txt 15639 15640 Sibirien
    programming_historian_copy/place_texts.txt 15653 15654 Krim
    programming_historian_copy/place_texts.txt 15667 15668 Krim
    programming_historian_copy/place_texts.txt 15685 15686 Siwasch
    programming_historian_copy/place_texts.txt 15689 15690 Kertsch
    programming_historian_copy/place_texts.txt 15956 15957 Selenodolsk
    programming_historian_copy/place_texts.txt 16021 16022 Selenodolsk
    programming_historian_copy/place_texts.txt 16150 16151 Selenodolsk
    programming_historian_copy/place_texts.txt 16201 16202 Selenodolsk
    programming_historian_copy/place_texts.txt 16257 16258 Selenodolsk
    programming_historian_copy/place_texts.txt 16291 16292 Selenodolsk
    programming_historian_copy/place_texts.txt 16407 16408 Selenodolsk
    programming_historian_copy/place_texts.txt 16512 16513 Jelabuga
    programming_historian_copy/place_texts.txt 16536 16537 Jelabuga
    programming_historian_copy/place_texts.txt 16550 16551 Jelabuga
    programming_historian_copy/place_texts.txt 16571 16572 Selenodolsk
    programming_historian_copy/place_texts.txt 16580 16581 Jelabuga
    programming_historian_copy/place_texts.txt 16598 16599 Selenodolsk
    programming_historian_copy/place_texts.txt 16611 16612 Selenodolsk
    programming_historian_copy/place_texts.txt 16628 16629 Moskau
    programming_historian_copy/place_texts.txt 16639 16640 Jarzewo
    programming_historian_copy/place_texts.txt 16652 16653 Merefa
    programming_historian_copy/place_texts.txt 16677 16678 Charkow
    programming_historian_copy/place_texts.txt 16779 16780 Jelabuga
    programming_historian_copy/place_texts.txt 16863 16864 Moskau
    programming_historian_copy/place_texts.txt 16865 16866 Workuta
    programming_historian_copy/place_texts.txt 17127 17128 Moskau
    programming_historian_copy/place_texts.txt 17136 17137 Lubjanka
    programming_historian_copy/place_texts.txt 17140 17141 Butyrka
    programming_historian_copy/place_texts.txt 17159 17160 Moskau
    programming_historian_copy/place_texts.txt 17162 17163 Workuta
    programming_historian_copy/place_texts.txt 17249 17250 Karaganda
    programming_historian_copy/place_texts.txt 17251 17252 Workuta
    programming_historian_copy/place_texts.txt 17256 17257 Karaganda
    programming_historian_copy/place_texts.txt 17269 17270 Workuta
    programming_historian_copy/place_texts.txt 17338 17339 Workuta
    programming_historian_copy/place_texts.txt 17477 17478 Moskau
    programming_historian_copy/place_texts.txt 17660 17661 Workuta
    programming_historian_copy/place_texts.txt 17684 17685 Stalingrad
    programming_historian_copy/place_texts.txt 17805 17806 Moskau
    programming_historian_copy/place_texts.txt 17810 17811 USA
    programming_historian_copy/place_texts.txt 17877 17878 Jelabuga
    programming_historian_copy/place_texts.txt 18119 18120 Gorodischtsche
    programming_historian_copy/place_texts.txt 18213 18214 Stalingrad
    programming_historian_copy/place_texts.txt 18243 18244 Stalingrad
    programming_historian_copy/place_texts.txt 18276 18277 Stalingrad
    programming_historian_copy/place_texts.txt 18331 18332 Kasan
    programming_historian_copy/place_texts.txt 18359 18360 Jelschanka
    programming_historian_copy/place_texts.txt 18428 18429 Jelschanka
    programming_historian_copy/place_texts.txt 18455 18456 Gorodischtsche
    programming_historian_copy/place_texts.txt 18471 18472 Jelschanka
    programming_historian_copy/place_texts.txt 18540 18541 Stalingrad
    programming_historian_copy/place_texts.txt 18836 18837 Johannes
    programming_historian_copy/place_texts.txt 18877 18878 Krasnodar
    programming_historian_copy/place_texts.txt 18882 18883 Charkow
    programming_historian_copy/place_texts.txt 18899 18900 Minsk
    programming_historian_copy/place_texts.txt 18904 18905 Kiew
    programming_historian_copy/place_texts.txt 18907 18908 Jar
    programming_historian_copy/place_texts.txt 18958 18959 Nikolajew
    programming_historian_copy/place_texts.txt 19020 19021 Johannes
    programming_historian_copy/place_texts.txt 19024 19025 Stalingrad
    programming_historian_copy/place_texts.txt 19030 19031 Stalingrad
    programming_historian_copy/place_texts.txt 19261 19262 Jelabuga
    programming_historian_copy/place_texts.txt 20017 20018 ar
    programming_historian_copy/place_texts.txt 20370 20371 Stalingrad
    programming_historian_copy/place_texts.txt 20411 20412 Stalingrad
    programming_historian_copy/place_texts.txt 20452 20453 Stalingrad
    programming_historian_copy/place_texts.txt 20529 20530 Stalingrad
    programming_historian_copy/place_texts.txt 20591 20592 Stalingrad
    programming_historian_copy/place_texts.txt 20640 20641 Stalingrad
    programming_historian_copy/place_texts.txt 20739 20740 Stalingrad
    programming_historian_copy/place_texts.txt 20761 20762 Stalingrad
    programming_historian_copy/place_texts.txt 20818 20819 Russland
    programming_historian_copy/place_texts.txt 20906 20907 Stalingrad
    programming_historian_copy/place_texts.txt 20941 20942 Russland
    programming_historian_copy/place_texts.txt 20963 20964 Stalingrad
    programming_historian_copy/place_texts.txt 21021 21022 Stalingrad
    programming_historian_copy/place_texts.txt 21069 21070 Jelabuga
    programming_historian_copy/place_texts.txt 21089 21090 Stalingrad
    programming_historian_copy/place_texts.txt 21233 21234 Stalingrad
    programming_historian_copy/place_texts.txt 21284 21285 Stalingrad
    programming_historian_copy/place_texts.txt 21897 21898 Adler
    programming_historian_copy/place_texts.txt 22069 22070 Adler
    programming_historian_copy/place_texts.txt 22083 22084 Adler
    programming_historian_copy/place_texts.txt 22253 22254 Kursk
    programming_historian_copy/place_texts.txt 22333 22334 Wjasma
    programming_historian_copy/place_texts.txt 22335 22336 Gshatsk
    programming_historian_copy/place_texts.txt 22339 22340 Smolensk
    programming_historian_copy/place_texts.txt 22350 22351 Kursk
    programming_historian_copy/place_texts.txt 22371 22372 Smolensk
    programming_historian_copy/place_texts.txt 22394 22395 Smolensk
    programming_historian_copy/place_texts.txt 22396 22397 Roslawl
    programming_historian_copy/place_texts.txt 22410 22411 Roslawl
    programming_historian_copy/place_texts.txt 22448 22449 Smolensk
    programming_historian_copy/place_texts.txt 22450 22451 Roslawl
    programming_historian_copy/place_texts.txt 22477 22478 Smolensk
    programming_historian_copy/place_texts.txt 22500 22501 Gomel
    programming_historian_copy/place_texts.txt 22570 22571 Witebsk
    programming_historian_copy/place_texts.txt 22737 22738 Witebsk
    programming_historian_copy/place_texts.txt 22739 22740 Borissow
    programming_historian_copy/place_texts.txt 22741 22742 Bobruisk
    programming_historian_copy/place_texts.txt 23209 23210 Moskau
    programming_historian_copy/place_texts.txt 23413 23414 Stalingrad
    programming_historian_copy/place_texts.txt 23506 23507 Stalingrad
    programming_historian_copy/place_texts.txt 23790 23791 Dwiri
    programming_historian_copy/place_texts.txt 23795 23796 Georgien
    programming_historian_copy/place_texts.txt 23996 23997 Jelabuga
    programming_historian_copy/place_texts.txt 24018 24019 Jelabuga
    programming_historian_copy/place_texts.txt 24077 24078 Stalingrad
    programming_historian_copy/place_texts.txt 24103 24104 Kasan
    programming_historian_copy/place_texts.txt 24173 24174 Jelabuga
    programming_historian_copy/place_texts.txt 24321 24322 Stalingrad
    programming_historian_copy/place_texts.txt 24329 24330 Stalingrad
    programming_historian_copy/place_texts.txt 24486 24487 Jelabuga
    programming_historian_copy/place_texts.txt 24529 24530 Jelabuga
    programming_historian_copy/place_texts.txt 24538 24539 Kasan
    programming_historian_copy/place_texts.txt 24932 24933 Stalingrad
    programming_historian_copy/place_texts.txt 24991 24992 Workuta
    programming_historian_copy/place_texts.txt 24993 24994 Astrachan
    programming_historian_copy/place_texts.txt 25029 25030 Stalingrad
    programming_historian_copy/place_texts.txt 25072 25073 Johannes
    programming_historian_copy/place_texts.txt 25605 25606 jelabuga
    programming_historian_copy/place_texts.txt 25638 25639 Selenodolsk
    programming_historian_copy/place_texts.txt 25670 25671 Jelabuga
    programming_historian_copy/place_texts.txt 25688 25689 Moskau
    programming_historian_copy/place_texts.txt 25690 25691 Jarzewo
    programming_historian_copy/place_texts.txt 25696 25697 Moskau
    programming_historian_copy/place_texts.txt 25702 25703 Minsk
    programming_historian_copy/place_texts.txt 25717 25718 Jarzewo
    programming_historian_copy/place_texts.txt 25763 25764 Merefa
    programming_historian_copy/place_texts.txt 25767 25768 Charkow
    programming_historian_copy/place_texts.txt 26099 26100 Jelabuga
    programming_historian_copy/place_texts.txt 26101 26102 Selenodolsk
    programming_historian_copy/place_texts.txt 26108 26109 Jelabuga
    programming_historian_copy/place_texts.txt 26391 26392 Stalingrad
    programming_historian_copy/place_texts.txt 26407 26408 Stalingrad
    programming_historian_copy/place_texts.txt 26440 26441 Stalingrad
    programming_historian_copy/place_texts.txt 26518 26519 Kasan
    programming_historian_copy/place_texts.txt 26533 26534 Stalingrad
    programming_historian_copy/place_texts.txt 26688 26689 Pensa
    programming_historian_copy/place_texts.txt 26694 26695 Pensa
    programming_historian_copy/place_texts.txt 26724 26725 Moskau
    programming_historian_copy/place_texts.txt 26737 26738 Pensa
    programming_historian_copy/place_texts.txt 26762 26763 Moskau
    programming_historian_copy/place_texts.txt 26825 26826 Moskau
    programming_historian_copy/place_texts.txt 26827 26828 Kuibyschew
    programming_historian_copy/place_texts.txt 26838 26839 Kuibyschew
    programming_historian_copy/place_texts.txt 26860 26861 Moskau
    programming_historian_copy/place_texts.txt 26898 26899 Stalingrad
    programming_historian_copy/place_texts.txt 26909 26910 Pensa
    programming_historian_copy/place_texts.txt 26911 26912 Kuibyschew
    programming_historian_copy/place_texts.txt 26990 26991 Jelabuga
    programming_historian_copy/place_texts.txt 27096 27097 Moskau
    programming_historian_copy/place_texts.txt 27160 27161 Jelabuga
    programming_historian_copy/place_texts.txt 27165 27166 Kasan
    programming_historian_copy/place_texts.txt 27167 27168 Pensa
    programming_historian_copy/place_texts.txt 27337 27338 Ukraine
    programming_historian_copy/place_texts.txt 27366 27367 Fastow
    programming_historian_copy/place_texts.txt 27368 27369 Shitomir
    programming_historian_copy/place_texts.txt 27372 27373 Owrutsch
    programming_historian_copy/place_texts.txt 27484 27485 Kursk
    programming_historian_copy/place_texts.txt 27486 27487 Orjol
    programming_historian_copy/place_texts.txt 27561 27562 Kiew
    programming_historian_copy/place_texts.txt 27575 27576 Kiew
    programming_historian_copy/place_texts.txt 27775 27776 Pronja
    programming_historian_copy/place_texts.txt 27804 27805 Gomel
    programming_historian_copy/place_texts.txt 27826 27827 Bobruisk
    programming_historian_copy/place_texts.txt 27837 27838 Moskau
    programming_historian_copy/place_texts.txt 27846 27847 Gomel
    programming_historian_copy/place_texts.txt 27853 27854 Retschiza
    programming_historian_copy/place_texts.txt 27864 27865 Shlobin
    programming_historian_copy/place_texts.txt 27866 27867 Mosyr
    programming_historian_copy/place_texts.txt 27885 27886 Retschiza
    programming_historian_copy/place_texts.txt 28076 28077 Krasnogorsk
    programming_historian_copy/place_texts.txt 28122 28123 Stalingrad
    programming_historian_copy/place_texts.txt 28126 28127 Kameschkowo
    programming_historian_copy/place_texts.txt 28184 28185 Stalingrad
    programming_historian_copy/place_texts.txt 28382 28383 Stalingrad
    programming_historian_copy/place_texts.txt 28397 28398 Stalingrad
    programming_historian_copy/place_texts.txt 28403 28404 Kameschkowo
    programming_historian_copy/place_texts.txt 28489 28490 Kasan
    programming_historian_copy/place_texts.txt 28504 28505 Kameschkowo
    programming_historian_copy/place_texts.txt 28659 28660 Stalingrad
    programming_historian_copy/place_texts.txt 28684 28685 STALINGRAD



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

    Stalingrad 100
    Moskau 55
    Jelabuga 30
    Selenodolsk 26
    Kasan 25
    Smolensk 16
    Workuta 11
    Russland 8
    Gorki 8
    Odessa 8


#### Application of Named Entity Recognition Tools.
* Important to assess results for validty, since they are machine-evaluated and prone to error.


```python
# Downloads "de_core_news_sm" pre-trained model.
!python -m spacy download de_core_news_sm

# Loads "de_core_news_sm" model for use in program into "language object" (nlp).
import spacy
nlp = spacy.load("de_core_news_sm")
```

    Traceback (most recent call last):
      File "/Users/kevin/opt/anaconda3/lib/python3.9/site-packages/urllib3/connection.py", line 174, in _new_conn
        conn = connection.create_connection(
      File "/Users/kevin/opt/anaconda3/lib/python3.9/site-packages/urllib3/util/connection.py", line 72, in create_connection
        for res in socket.getaddrinfo(host, port, family, socket.SOCK_STREAM):
      File "/Users/kevin/opt/anaconda3/lib/python3.9/socket.py", line 954, in getaddrinfo
        for res in _socket.getaddrinfo(host, port, family, type, proto, flags):
    socket.gaierror: [Errno 8] nodename nor servname provided, or not known
    
    During handling of the above exception, another exception occurred:
    
    Traceback (most recent call last):
      File "/Users/kevin/opt/anaconda3/lib/python3.9/site-packages/urllib3/connectionpool.py", line 703, in urlopen
        httplib_response = self._make_request(
      File "/Users/kevin/opt/anaconda3/lib/python3.9/site-packages/urllib3/connectionpool.py", line 386, in _make_request
        self._validate_conn(conn)
      File "/Users/kevin/opt/anaconda3/lib/python3.9/site-packages/urllib3/connectionpool.py", line 1040, in _validate_conn
        conn.connect()
      File "/Users/kevin/opt/anaconda3/lib/python3.9/site-packages/urllib3/connection.py", line 358, in connect
        self.sock = conn = self._new_conn()
      File "/Users/kevin/opt/anaconda3/lib/python3.9/site-packages/urllib3/connection.py", line 186, in _new_conn
        raise NewConnectionError(
    urllib3.exceptions.NewConnectionError: <urllib3.connection.HTTPSConnection object at 0x7f9469d391f0>: Failed to establish a new connection: [Errno 8] nodename nor servname provided, or not known
    
    During handling of the above exception, another exception occurred:
    
    Traceback (most recent call last):
      File "/Users/kevin/opt/anaconda3/lib/python3.9/site-packages/requests/adapters.py", line 440, in send
        resp = conn.urlopen(
      File "/Users/kevin/opt/anaconda3/lib/python3.9/site-packages/urllib3/connectionpool.py", line 785, in urlopen
        retries = retries.increment(
      File "/Users/kevin/opt/anaconda3/lib/python3.9/site-packages/urllib3/util/retry.py", line 592, in increment
        raise MaxRetryError(_pool, url, error or ResponseError(cause))
    urllib3.exceptions.MaxRetryError: HTTPSConnectionPool(host='raw.githubusercontent.com', port=443): Max retries exceeded with url: /explosion/spacy-models/master/compatibility.json (Caused by NewConnectionError('<urllib3.connection.HTTPSConnection object at 0x7f9469d391f0>: Failed to establish a new connection: [Errno 8] nodename nor servname provided, or not known'))
    
    During handling of the above exception, another exception occurred:
    
    Traceback (most recent call last):
      File "/Users/kevin/opt/anaconda3/lib/python3.9/runpy.py", line 197, in _run_module_as_main
        return _run_code(code, main_globals, None,
      File "/Users/kevin/opt/anaconda3/lib/python3.9/runpy.py", line 87, in _run_code
        exec(code, run_globals)
      File "/Users/kevin/opt/anaconda3/lib/python3.9/site-packages/spacy/__main__.py", line 4, in <module>
        setup_cli()
      File "/Users/kevin/opt/anaconda3/lib/python3.9/site-packages/spacy/cli/_util.py", line 71, in setup_cli
        command(prog_name=COMMAND)
      File "/Users/kevin/opt/anaconda3/lib/python3.9/site-packages/click/core.py", line 1128, in __call__
        return self.main(*args, **kwargs)
      File "/Users/kevin/opt/anaconda3/lib/python3.9/site-packages/click/core.py", line 1053, in main
        rv = self.invoke(ctx)
      File "/Users/kevin/opt/anaconda3/lib/python3.9/site-packages/click/core.py", line 1659, in invoke
        return _process_result(sub_ctx.command.invoke(sub_ctx))
      File "/Users/kevin/opt/anaconda3/lib/python3.9/site-packages/click/core.py", line 1395, in invoke
        return ctx.invoke(self.callback, **ctx.params)
      File "/Users/kevin/opt/anaconda3/lib/python3.9/site-packages/click/core.py", line 754, in invoke
        return __callback(*args, **kwargs)
      File "/Users/kevin/opt/anaconda3/lib/python3.9/site-packages/typer/main.py", line 532, in wrapper
        return callback(**use_params)  # type: ignore
      File "/Users/kevin/opt/anaconda3/lib/python3.9/site-packages/spacy/cli/download.py", line 35, in download_cli
        download(model, direct, sdist, *ctx.args)
      File "/Users/kevin/opt/anaconda3/lib/python3.9/site-packages/spacy/cli/download.py", line 67, in download
        compatibility = get_compatibility()
      File "/Users/kevin/opt/anaconda3/lib/python3.9/site-packages/spacy/cli/download.py", line 78, in get_compatibility
        r = requests.get(about.__compatibility__)
      File "/Users/kevin/opt/anaconda3/lib/python3.9/site-packages/requests/api.py", line 75, in get
        return request('get', url, params=params, **kwargs)
      File "/Users/kevin/opt/anaconda3/lib/python3.9/site-packages/requests/api.py", line 61, in request
        return session.request(method=method, url=url, **kwargs)
      File "/Users/kevin/opt/anaconda3/lib/python3.9/site-packages/requests/sessions.py", line 529, in request
        resp = self.send(prep, **send_kwargs)
      File "/Users/kevin/opt/anaconda3/lib/python3.9/site-packages/requests/sessions.py", line 645, in send
        r = adapter.send(request, **kwargs)
      File "/Users/kevin/opt/anaconda3/lib/python3.9/site-packages/requests/adapters.py", line 519, in send
        raise ConnectionError(e, request=request)
    requests.exceptions.ConnectionError: HTTPSConnectionPool(host='raw.githubusercontent.com', port=443): Max retries exceeded with url: /explosion/spacy-models/master/compatibility.json (Caused by NewConnectionError('<urllib3.connection.HTTPSConnection object at 0x7f9469d391f0>: Failed to establish a new connection: [Errno 8] nodename nor servname provided, or not known'))



```python
# Initialization of variable with model-processed text in German.
doc = nlp("Karl-Heinz Quade ist von März 1944 bis August 1948 im Lager 150 in Grjasowez interniert.")

# Printing of those locations identified by model in provided text.
for ent in doc.ents:
    print(ent.text, ent.label_, ent.start, ent.end)
```

    Karl-Heinz Quade PER 0 2
    Grjasowez LOC 13 14


#### Visualization of Index-Locations Pertaining to Matched Terms Along Text


```python
# Use of imported displaCy tool to visualize locations of matched terms along text.
from spacy import displacy
displacy.render(doc, jupyter = True, style = "ent")
```


<span class="tex2jax_ignore"><div class="entities" style="line-height: 2.5; direction: ltr">
<mark class="entity" style="background: #ddd; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Karl-Heinz Quade
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">PER</span>
</mark>
 ist von März 1944 bis August 1948 im Lager 150 in 
<mark class="entity" style="background: #ff9561; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Grjasowez
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">LOC</span>
</mark>
 interniert.</div></span>


#### Visualization of Relations Between Tokens Along Text


```python
# Use of displaCy tool to visualize relations between tokens along text.
displacy.render(doc, jupyter = True, style = "dep")
```


<span class="tex2jax_ignore"><svg xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" xml:lang="de" id="b25fa8adc792408ba2a48ddb8bc0c764-0" class="displacy" width="2675" height="662.0" direction="ltr" style="max-width: none; height: 662.0px; color: #000000; background: #ffffff; font-family: Arial; direction: ltr">
<text class="displacy-token" fill="currentColor" text-anchor="middle" y="572.0">
    <tspan class="displacy-word" fill="currentColor" x="50">Karl-Heinz</tspan>
    <tspan class="displacy-tag" dy="2em" fill="currentColor" x="50">PROPN</tspan>
</text>

<text class="displacy-token" fill="currentColor" text-anchor="middle" y="572.0">
    <tspan class="displacy-word" fill="currentColor" x="225">Quade</tspan>
    <tspan class="displacy-tag" dy="2em" fill="currentColor" x="225">PROPN</tspan>
</text>

<text class="displacy-token" fill="currentColor" text-anchor="middle" y="572.0">
    <tspan class="displacy-word" fill="currentColor" x="400">ist</tspan>
    <tspan class="displacy-tag" dy="2em" fill="currentColor" x="400">AUX</tspan>
</text>

<text class="displacy-token" fill="currentColor" text-anchor="middle" y="572.0">
    <tspan class="displacy-word" fill="currentColor" x="575">von</tspan>
    <tspan class="displacy-tag" dy="2em" fill="currentColor" x="575">ADP</tspan>
</text>

<text class="displacy-token" fill="currentColor" text-anchor="middle" y="572.0">
    <tspan class="displacy-word" fill="currentColor" x="750">März</tspan>
    <tspan class="displacy-tag" dy="2em" fill="currentColor" x="750">NOUN</tspan>
</text>

<text class="displacy-token" fill="currentColor" text-anchor="middle" y="572.0">
    <tspan class="displacy-word" fill="currentColor" x="925">1944</tspan>
    <tspan class="displacy-tag" dy="2em" fill="currentColor" x="925">NUM</tspan>
</text>

<text class="displacy-token" fill="currentColor" text-anchor="middle" y="572.0">
    <tspan class="displacy-word" fill="currentColor" x="1100">bis</tspan>
    <tspan class="displacy-tag" dy="2em" fill="currentColor" x="1100">ADP</tspan>
</text>

<text class="displacy-token" fill="currentColor" text-anchor="middle" y="572.0">
    <tspan class="displacy-word" fill="currentColor" x="1275">August</tspan>
    <tspan class="displacy-tag" dy="2em" fill="currentColor" x="1275">NOUN</tspan>
</text>

<text class="displacy-token" fill="currentColor" text-anchor="middle" y="572.0">
    <tspan class="displacy-word" fill="currentColor" x="1450">1948</tspan>
    <tspan class="displacy-tag" dy="2em" fill="currentColor" x="1450">NUM</tspan>
</text>

<text class="displacy-token" fill="currentColor" text-anchor="middle" y="572.0">
    <tspan class="displacy-word" fill="currentColor" x="1625">im</tspan>
    <tspan class="displacy-tag" dy="2em" fill="currentColor" x="1625">ADP</tspan>
</text>

<text class="displacy-token" fill="currentColor" text-anchor="middle" y="572.0">
    <tspan class="displacy-word" fill="currentColor" x="1800">Lager</tspan>
    <tspan class="displacy-tag" dy="2em" fill="currentColor" x="1800">NOUN</tspan>
</text>

<text class="displacy-token" fill="currentColor" text-anchor="middle" y="572.0">
    <tspan class="displacy-word" fill="currentColor" x="1975">150</tspan>
    <tspan class="displacy-tag" dy="2em" fill="currentColor" x="1975">NUM</tspan>
</text>

<text class="displacy-token" fill="currentColor" text-anchor="middle" y="572.0">
    <tspan class="displacy-word" fill="currentColor" x="2150">in</tspan>
    <tspan class="displacy-tag" dy="2em" fill="currentColor" x="2150">ADP</tspan>
</text>

<text class="displacy-token" fill="currentColor" text-anchor="middle" y="572.0">
    <tspan class="displacy-word" fill="currentColor" x="2325">Grjasowez</tspan>
    <tspan class="displacy-tag" dy="2em" fill="currentColor" x="2325">PROPN</tspan>
</text>

<text class="displacy-token" fill="currentColor" text-anchor="middle" y="572.0">
    <tspan class="displacy-word" fill="currentColor" x="2500">interniert.</tspan>
    <tspan class="displacy-tag" dy="2em" fill="currentColor" x="2500">VERB</tspan>
</text>

<g class="displacy-arrow">
    <path class="displacy-arc" id="arrow-b25fa8adc792408ba2a48ddb8bc0c764-0-0" stroke-width="2px" d="M70,527.0 C70,439.5 200.0,439.5 200.0,527.0" fill="none" stroke="currentColor"/>
    <text dy="1.25em" style="font-size: 0.8em; letter-spacing: 1px">
        <textPath xlink:href="#arrow-b25fa8adc792408ba2a48ddb8bc0c764-0-0" class="displacy-label" startOffset="50%" side="left" fill="currentColor" text-anchor="middle">pnc</textPath>
    </text>
    <path class="displacy-arrowhead" d="M70,529.0 L62,517.0 78,517.0" fill="currentColor"/>
</g>

<g class="displacy-arrow">
    <path class="displacy-arc" id="arrow-b25fa8adc792408ba2a48ddb8bc0c764-0-1" stroke-width="2px" d="M245,527.0 C245,439.5 375.0,439.5 375.0,527.0" fill="none" stroke="currentColor"/>
    <text dy="1.25em" style="font-size: 0.8em; letter-spacing: 1px">
        <textPath xlink:href="#arrow-b25fa8adc792408ba2a48ddb8bc0c764-0-1" class="displacy-label" startOffset="50%" side="left" fill="currentColor" text-anchor="middle">sb</textPath>
    </text>
    <path class="displacy-arrowhead" d="M245,529.0 L237,517.0 253,517.0" fill="currentColor"/>
</g>

<g class="displacy-arrow">
    <path class="displacy-arc" id="arrow-b25fa8adc792408ba2a48ddb8bc0c764-0-2" stroke-width="2px" d="M595,527.0 C595,89.5 2495.0,89.5 2495.0,527.0" fill="none" stroke="currentColor"/>
    <text dy="1.25em" style="font-size: 0.8em; letter-spacing: 1px">
        <textPath xlink:href="#arrow-b25fa8adc792408ba2a48ddb8bc0c764-0-2" class="displacy-label" startOffset="50%" side="left" fill="currentColor" text-anchor="middle">mo</textPath>
    </text>
    <path class="displacy-arrowhead" d="M595,529.0 L587,517.0 603,517.0" fill="currentColor"/>
</g>

<g class="displacy-arrow">
    <path class="displacy-arc" id="arrow-b25fa8adc792408ba2a48ddb8bc0c764-0-3" stroke-width="2px" d="M595,527.0 C595,439.5 725.0,439.5 725.0,527.0" fill="none" stroke="currentColor"/>
    <text dy="1.25em" style="font-size: 0.8em; letter-spacing: 1px">
        <textPath xlink:href="#arrow-b25fa8adc792408ba2a48ddb8bc0c764-0-3" class="displacy-label" startOffset="50%" side="left" fill="currentColor" text-anchor="middle">nk</textPath>
    </text>
    <path class="displacy-arrowhead" d="M725.0,529.0 L733.0,517.0 717.0,517.0" fill="currentColor"/>
</g>

<g class="displacy-arrow">
    <path class="displacy-arc" id="arrow-b25fa8adc792408ba2a48ddb8bc0c764-0-4" stroke-width="2px" d="M770,527.0 C770,439.5 900.0,439.5 900.0,527.0" fill="none" stroke="currentColor"/>
    <text dy="1.25em" style="font-size: 0.8em; letter-spacing: 1px">
        <textPath xlink:href="#arrow-b25fa8adc792408ba2a48ddb8bc0c764-0-4" class="displacy-label" startOffset="50%" side="left" fill="currentColor" text-anchor="middle">nk</textPath>
    </text>
    <path class="displacy-arrowhead" d="M900.0,529.0 L908.0,517.0 892.0,517.0" fill="currentColor"/>
</g>

<g class="displacy-arrow">
    <path class="displacy-arc" id="arrow-b25fa8adc792408ba2a48ddb8bc0c764-0-5" stroke-width="2px" d="M1120,527.0 C1120,177.0 2490.0,177.0 2490.0,527.0" fill="none" stroke="currentColor"/>
    <text dy="1.25em" style="font-size: 0.8em; letter-spacing: 1px">
        <textPath xlink:href="#arrow-b25fa8adc792408ba2a48ddb8bc0c764-0-5" class="displacy-label" startOffset="50%" side="left" fill="currentColor" text-anchor="middle">mo</textPath>
    </text>
    <path class="displacy-arrowhead" d="M1120,529.0 L1112,517.0 1128,517.0" fill="currentColor"/>
</g>

<g class="displacy-arrow">
    <path class="displacy-arc" id="arrow-b25fa8adc792408ba2a48ddb8bc0c764-0-6" stroke-width="2px" d="M1120,527.0 C1120,439.5 1250.0,439.5 1250.0,527.0" fill="none" stroke="currentColor"/>
    <text dy="1.25em" style="font-size: 0.8em; letter-spacing: 1px">
        <textPath xlink:href="#arrow-b25fa8adc792408ba2a48ddb8bc0c764-0-6" class="displacy-label" startOffset="50%" side="left" fill="currentColor" text-anchor="middle">nk</textPath>
    </text>
    <path class="displacy-arrowhead" d="M1250.0,529.0 L1258.0,517.0 1242.0,517.0" fill="currentColor"/>
</g>

<g class="displacy-arrow">
    <path class="displacy-arc" id="arrow-b25fa8adc792408ba2a48ddb8bc0c764-0-7" stroke-width="2px" d="M1295,527.0 C1295,439.5 1425.0,439.5 1425.0,527.0" fill="none" stroke="currentColor"/>
    <text dy="1.25em" style="font-size: 0.8em; letter-spacing: 1px">
        <textPath xlink:href="#arrow-b25fa8adc792408ba2a48ddb8bc0c764-0-7" class="displacy-label" startOffset="50%" side="left" fill="currentColor" text-anchor="middle">nk</textPath>
    </text>
    <path class="displacy-arrowhead" d="M1425.0,529.0 L1433.0,517.0 1417.0,517.0" fill="currentColor"/>
</g>

<g class="displacy-arrow">
    <path class="displacy-arc" id="arrow-b25fa8adc792408ba2a48ddb8bc0c764-0-8" stroke-width="2px" d="M1645,527.0 C1645,264.5 2485.0,264.5 2485.0,527.0" fill="none" stroke="currentColor"/>
    <text dy="1.25em" style="font-size: 0.8em; letter-spacing: 1px">
        <textPath xlink:href="#arrow-b25fa8adc792408ba2a48ddb8bc0c764-0-8" class="displacy-label" startOffset="50%" side="left" fill="currentColor" text-anchor="middle">mo</textPath>
    </text>
    <path class="displacy-arrowhead" d="M1645,529.0 L1637,517.0 1653,517.0" fill="currentColor"/>
</g>

<g class="displacy-arrow">
    <path class="displacy-arc" id="arrow-b25fa8adc792408ba2a48ddb8bc0c764-0-9" stroke-width="2px" d="M1645,527.0 C1645,439.5 1775.0,439.5 1775.0,527.0" fill="none" stroke="currentColor"/>
    <text dy="1.25em" style="font-size: 0.8em; letter-spacing: 1px">
        <textPath xlink:href="#arrow-b25fa8adc792408ba2a48ddb8bc0c764-0-9" class="displacy-label" startOffset="50%" side="left" fill="currentColor" text-anchor="middle">nk</textPath>
    </text>
    <path class="displacy-arrowhead" d="M1775.0,529.0 L1783.0,517.0 1767.0,517.0" fill="currentColor"/>
</g>

<g class="displacy-arrow">
    <path class="displacy-arc" id="arrow-b25fa8adc792408ba2a48ddb8bc0c764-0-10" stroke-width="2px" d="M1820,527.0 C1820,439.5 1950.0,439.5 1950.0,527.0" fill="none" stroke="currentColor"/>
    <text dy="1.25em" style="font-size: 0.8em; letter-spacing: 1px">
        <textPath xlink:href="#arrow-b25fa8adc792408ba2a48ddb8bc0c764-0-10" class="displacy-label" startOffset="50%" side="left" fill="currentColor" text-anchor="middle">nk</textPath>
    </text>
    <path class="displacy-arrowhead" d="M1950.0,529.0 L1958.0,517.0 1942.0,517.0" fill="currentColor"/>
</g>

<g class="displacy-arrow">
    <path class="displacy-arc" id="arrow-b25fa8adc792408ba2a48ddb8bc0c764-0-11" stroke-width="2px" d="M2170,527.0 C2170,352.0 2480.0,352.0 2480.0,527.0" fill="none" stroke="currentColor"/>
    <text dy="1.25em" style="font-size: 0.8em; letter-spacing: 1px">
        <textPath xlink:href="#arrow-b25fa8adc792408ba2a48ddb8bc0c764-0-11" class="displacy-label" startOffset="50%" side="left" fill="currentColor" text-anchor="middle">mo</textPath>
    </text>
    <path class="displacy-arrowhead" d="M2170,529.0 L2162,517.0 2178,517.0" fill="currentColor"/>
</g>

<g class="displacy-arrow">
    <path class="displacy-arc" id="arrow-b25fa8adc792408ba2a48ddb8bc0c764-0-12" stroke-width="2px" d="M2170,527.0 C2170,439.5 2300.0,439.5 2300.0,527.0" fill="none" stroke="currentColor"/>
    <text dy="1.25em" style="font-size: 0.8em; letter-spacing: 1px">
        <textPath xlink:href="#arrow-b25fa8adc792408ba2a48ddb8bc0c764-0-12" class="displacy-label" startOffset="50%" side="left" fill="currentColor" text-anchor="middle">nk</textPath>
    </text>
    <path class="displacy-arrowhead" d="M2300.0,529.0 L2308.0,517.0 2292.0,517.0" fill="currentColor"/>
</g>

<g class="displacy-arrow">
    <path class="displacy-arc" id="arrow-b25fa8adc792408ba2a48ddb8bc0c764-0-13" stroke-width="2px" d="M420,527.0 C420,2.0 2500.0,2.0 2500.0,527.0" fill="none" stroke="currentColor"/>
    <text dy="1.25em" style="font-size: 0.8em; letter-spacing: 1px">
        <textPath xlink:href="#arrow-b25fa8adc792408ba2a48ddb8bc0c764-0-13" class="displacy-label" startOffset="50%" side="left" fill="currentColor" text-anchor="middle">oc</textPath>
    </text>
    <path class="displacy-arrowhead" d="M2500.0,529.0 L2508.0,517.0 2492.0,517.0" fill="currentColor"/>
</g>
</svg></span>



```python
from pathlib import Path

# Attempt at saving displacy visualizations to a file for use elsewhere.
svg = displacy.render(doc, style="dep")
output_path = Path("sentence.svg")
output_path.write_text(str(svg))
```


<span class="tex2jax_ignore"><svg xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" xml:lang="de" id="451ec6f4a0c84f5ca7f770751d33982b-0" class="displacy" width="2675" height="662.0" direction="ltr" style="max-width: none; height: 662.0px; color: #000000; background: #ffffff; font-family: Arial; direction: ltr">
<text class="displacy-token" fill="currentColor" text-anchor="middle" y="572.0">
    <tspan class="displacy-word" fill="currentColor" x="50">Karl-Heinz</tspan>
    <tspan class="displacy-tag" dy="2em" fill="currentColor" x="50">PROPN</tspan>
</text>

<text class="displacy-token" fill="currentColor" text-anchor="middle" y="572.0">
    <tspan class="displacy-word" fill="currentColor" x="225">Quade</tspan>
    <tspan class="displacy-tag" dy="2em" fill="currentColor" x="225">PROPN</tspan>
</text>

<text class="displacy-token" fill="currentColor" text-anchor="middle" y="572.0">
    <tspan class="displacy-word" fill="currentColor" x="400">ist</tspan>
    <tspan class="displacy-tag" dy="2em" fill="currentColor" x="400">AUX</tspan>
</text>

<text class="displacy-token" fill="currentColor" text-anchor="middle" y="572.0">
    <tspan class="displacy-word" fill="currentColor" x="575">von</tspan>
    <tspan class="displacy-tag" dy="2em" fill="currentColor" x="575">ADP</tspan>
</text>

<text class="displacy-token" fill="currentColor" text-anchor="middle" y="572.0">
    <tspan class="displacy-word" fill="currentColor" x="750">März</tspan>
    <tspan class="displacy-tag" dy="2em" fill="currentColor" x="750">NOUN</tspan>
</text>

<text class="displacy-token" fill="currentColor" text-anchor="middle" y="572.0">
    <tspan class="displacy-word" fill="currentColor" x="925">1944</tspan>
    <tspan class="displacy-tag" dy="2em" fill="currentColor" x="925">NUM</tspan>
</text>

<text class="displacy-token" fill="currentColor" text-anchor="middle" y="572.0">
    <tspan class="displacy-word" fill="currentColor" x="1100">bis</tspan>
    <tspan class="displacy-tag" dy="2em" fill="currentColor" x="1100">ADP</tspan>
</text>

<text class="displacy-token" fill="currentColor" text-anchor="middle" y="572.0">
    <tspan class="displacy-word" fill="currentColor" x="1275">August</tspan>
    <tspan class="displacy-tag" dy="2em" fill="currentColor" x="1275">NOUN</tspan>
</text>

<text class="displacy-token" fill="currentColor" text-anchor="middle" y="572.0">
    <tspan class="displacy-word" fill="currentColor" x="1450">1948</tspan>
    <tspan class="displacy-tag" dy="2em" fill="currentColor" x="1450">NUM</tspan>
</text>

<text class="displacy-token" fill="currentColor" text-anchor="middle" y="572.0">
    <tspan class="displacy-word" fill="currentColor" x="1625">im</tspan>
    <tspan class="displacy-tag" dy="2em" fill="currentColor" x="1625">ADP</tspan>
</text>

<text class="displacy-token" fill="currentColor" text-anchor="middle" y="572.0">
    <tspan class="displacy-word" fill="currentColor" x="1800">Lager</tspan>
    <tspan class="displacy-tag" dy="2em" fill="currentColor" x="1800">NOUN</tspan>
</text>

<text class="displacy-token" fill="currentColor" text-anchor="middle" y="572.0">
    <tspan class="displacy-word" fill="currentColor" x="1975">150</tspan>
    <tspan class="displacy-tag" dy="2em" fill="currentColor" x="1975">NUM</tspan>
</text>

<text class="displacy-token" fill="currentColor" text-anchor="middle" y="572.0">
    <tspan class="displacy-word" fill="currentColor" x="2150">in</tspan>
    <tspan class="displacy-tag" dy="2em" fill="currentColor" x="2150">ADP</tspan>
</text>

<text class="displacy-token" fill="currentColor" text-anchor="middle" y="572.0">
    <tspan class="displacy-word" fill="currentColor" x="2325">Grjasowez</tspan>
    <tspan class="displacy-tag" dy="2em" fill="currentColor" x="2325">PROPN</tspan>
</text>

<text class="displacy-token" fill="currentColor" text-anchor="middle" y="572.0">
    <tspan class="displacy-word" fill="currentColor" x="2500">interniert.</tspan>
    <tspan class="displacy-tag" dy="2em" fill="currentColor" x="2500">VERB</tspan>
</text>

<g class="displacy-arrow">
    <path class="displacy-arc" id="arrow-451ec6f4a0c84f5ca7f770751d33982b-0-0" stroke-width="2px" d="M70,527.0 C70,439.5 200.0,439.5 200.0,527.0" fill="none" stroke="currentColor"/>
    <text dy="1.25em" style="font-size: 0.8em; letter-spacing: 1px">
        <textPath xlink:href="#arrow-451ec6f4a0c84f5ca7f770751d33982b-0-0" class="displacy-label" startOffset="50%" side="left" fill="currentColor" text-anchor="middle">pnc</textPath>
    </text>
    <path class="displacy-arrowhead" d="M70,529.0 L62,517.0 78,517.0" fill="currentColor"/>
</g>

<g class="displacy-arrow">
    <path class="displacy-arc" id="arrow-451ec6f4a0c84f5ca7f770751d33982b-0-1" stroke-width="2px" d="M245,527.0 C245,439.5 375.0,439.5 375.0,527.0" fill="none" stroke="currentColor"/>
    <text dy="1.25em" style="font-size: 0.8em; letter-spacing: 1px">
        <textPath xlink:href="#arrow-451ec6f4a0c84f5ca7f770751d33982b-0-1" class="displacy-label" startOffset="50%" side="left" fill="currentColor" text-anchor="middle">sb</textPath>
    </text>
    <path class="displacy-arrowhead" d="M245,529.0 L237,517.0 253,517.0" fill="currentColor"/>
</g>

<g class="displacy-arrow">
    <path class="displacy-arc" id="arrow-451ec6f4a0c84f5ca7f770751d33982b-0-2" stroke-width="2px" d="M595,527.0 C595,89.5 2495.0,89.5 2495.0,527.0" fill="none" stroke="currentColor"/>
    <text dy="1.25em" style="font-size: 0.8em; letter-spacing: 1px">
        <textPath xlink:href="#arrow-451ec6f4a0c84f5ca7f770751d33982b-0-2" class="displacy-label" startOffset="50%" side="left" fill="currentColor" text-anchor="middle">mo</textPath>
    </text>
    <path class="displacy-arrowhead" d="M595,529.0 L587,517.0 603,517.0" fill="currentColor"/>
</g>

<g class="displacy-arrow">
    <path class="displacy-arc" id="arrow-451ec6f4a0c84f5ca7f770751d33982b-0-3" stroke-width="2px" d="M595,527.0 C595,439.5 725.0,439.5 725.0,527.0" fill="none" stroke="currentColor"/>
    <text dy="1.25em" style="font-size: 0.8em; letter-spacing: 1px">
        <textPath xlink:href="#arrow-451ec6f4a0c84f5ca7f770751d33982b-0-3" class="displacy-label" startOffset="50%" side="left" fill="currentColor" text-anchor="middle">nk</textPath>
    </text>
    <path class="displacy-arrowhead" d="M725.0,529.0 L733.0,517.0 717.0,517.0" fill="currentColor"/>
</g>

<g class="displacy-arrow">
    <path class="displacy-arc" id="arrow-451ec6f4a0c84f5ca7f770751d33982b-0-4" stroke-width="2px" d="M770,527.0 C770,439.5 900.0,439.5 900.0,527.0" fill="none" stroke="currentColor"/>
    <text dy="1.25em" style="font-size: 0.8em; letter-spacing: 1px">
        <textPath xlink:href="#arrow-451ec6f4a0c84f5ca7f770751d33982b-0-4" class="displacy-label" startOffset="50%" side="left" fill="currentColor" text-anchor="middle">nk</textPath>
    </text>
    <path class="displacy-arrowhead" d="M900.0,529.0 L908.0,517.0 892.0,517.0" fill="currentColor"/>
</g>

<g class="displacy-arrow">
    <path class="displacy-arc" id="arrow-451ec6f4a0c84f5ca7f770751d33982b-0-5" stroke-width="2px" d="M1120,527.0 C1120,177.0 2490.0,177.0 2490.0,527.0" fill="none" stroke="currentColor"/>
    <text dy="1.25em" style="font-size: 0.8em; letter-spacing: 1px">
        <textPath xlink:href="#arrow-451ec6f4a0c84f5ca7f770751d33982b-0-5" class="displacy-label" startOffset="50%" side="left" fill="currentColor" text-anchor="middle">mo</textPath>
    </text>
    <path class="displacy-arrowhead" d="M1120,529.0 L1112,517.0 1128,517.0" fill="currentColor"/>
</g>

<g class="displacy-arrow">
    <path class="displacy-arc" id="arrow-451ec6f4a0c84f5ca7f770751d33982b-0-6" stroke-width="2px" d="M1120,527.0 C1120,439.5 1250.0,439.5 1250.0,527.0" fill="none" stroke="currentColor"/>
    <text dy="1.25em" style="font-size: 0.8em; letter-spacing: 1px">
        <textPath xlink:href="#arrow-451ec6f4a0c84f5ca7f770751d33982b-0-6" class="displacy-label" startOffset="50%" side="left" fill="currentColor" text-anchor="middle">nk</textPath>
    </text>
    <path class="displacy-arrowhead" d="M1250.0,529.0 L1258.0,517.0 1242.0,517.0" fill="currentColor"/>
</g>

<g class="displacy-arrow">
    <path class="displacy-arc" id="arrow-451ec6f4a0c84f5ca7f770751d33982b-0-7" stroke-width="2px" d="M1295,527.0 C1295,439.5 1425.0,439.5 1425.0,527.0" fill="none" stroke="currentColor"/>
    <text dy="1.25em" style="font-size: 0.8em; letter-spacing: 1px">
        <textPath xlink:href="#arrow-451ec6f4a0c84f5ca7f770751d33982b-0-7" class="displacy-label" startOffset="50%" side="left" fill="currentColor" text-anchor="middle">nk</textPath>
    </text>
    <path class="displacy-arrowhead" d="M1425.0,529.0 L1433.0,517.0 1417.0,517.0" fill="currentColor"/>
</g>

<g class="displacy-arrow">
    <path class="displacy-arc" id="arrow-451ec6f4a0c84f5ca7f770751d33982b-0-8" stroke-width="2px" d="M1645,527.0 C1645,264.5 2485.0,264.5 2485.0,527.0" fill="none" stroke="currentColor"/>
    <text dy="1.25em" style="font-size: 0.8em; letter-spacing: 1px">
        <textPath xlink:href="#arrow-451ec6f4a0c84f5ca7f770751d33982b-0-8" class="displacy-label" startOffset="50%" side="left" fill="currentColor" text-anchor="middle">mo</textPath>
    </text>
    <path class="displacy-arrowhead" d="M1645,529.0 L1637,517.0 1653,517.0" fill="currentColor"/>
</g>

<g class="displacy-arrow">
    <path class="displacy-arc" id="arrow-451ec6f4a0c84f5ca7f770751d33982b-0-9" stroke-width="2px" d="M1645,527.0 C1645,439.5 1775.0,439.5 1775.0,527.0" fill="none" stroke="currentColor"/>
    <text dy="1.25em" style="font-size: 0.8em; letter-spacing: 1px">
        <textPath xlink:href="#arrow-451ec6f4a0c84f5ca7f770751d33982b-0-9" class="displacy-label" startOffset="50%" side="left" fill="currentColor" text-anchor="middle">nk</textPath>
    </text>
    <path class="displacy-arrowhead" d="M1775.0,529.0 L1783.0,517.0 1767.0,517.0" fill="currentColor"/>
</g>

<g class="displacy-arrow">
    <path class="displacy-arc" id="arrow-451ec6f4a0c84f5ca7f770751d33982b-0-10" stroke-width="2px" d="M1820,527.0 C1820,439.5 1950.0,439.5 1950.0,527.0" fill="none" stroke="currentColor"/>
    <text dy="1.25em" style="font-size: 0.8em; letter-spacing: 1px">
        <textPath xlink:href="#arrow-451ec6f4a0c84f5ca7f770751d33982b-0-10" class="displacy-label" startOffset="50%" side="left" fill="currentColor" text-anchor="middle">nk</textPath>
    </text>
    <path class="displacy-arrowhead" d="M1950.0,529.0 L1958.0,517.0 1942.0,517.0" fill="currentColor"/>
</g>

<g class="displacy-arrow">
    <path class="displacy-arc" id="arrow-451ec6f4a0c84f5ca7f770751d33982b-0-11" stroke-width="2px" d="M2170,527.0 C2170,352.0 2480.0,352.0 2480.0,527.0" fill="none" stroke="currentColor"/>
    <text dy="1.25em" style="font-size: 0.8em; letter-spacing: 1px">
        <textPath xlink:href="#arrow-451ec6f4a0c84f5ca7f770751d33982b-0-11" class="displacy-label" startOffset="50%" side="left" fill="currentColor" text-anchor="middle">mo</textPath>
    </text>
    <path class="displacy-arrowhead" d="M2170,529.0 L2162,517.0 2178,517.0" fill="currentColor"/>
</g>

<g class="displacy-arrow">
    <path class="displacy-arc" id="arrow-451ec6f4a0c84f5ca7f770751d33982b-0-12" stroke-width="2px" d="M2170,527.0 C2170,439.5 2300.0,439.5 2300.0,527.0" fill="none" stroke="currentColor"/>
    <text dy="1.25em" style="font-size: 0.8em; letter-spacing: 1px">
        <textPath xlink:href="#arrow-451ec6f4a0c84f5ca7f770751d33982b-0-12" class="displacy-label" startOffset="50%" side="left" fill="currentColor" text-anchor="middle">nk</textPath>
    </text>
    <path class="displacy-arrowhead" d="M2300.0,529.0 L2308.0,517.0 2292.0,517.0" fill="currentColor"/>
</g>

<g class="displacy-arrow">
    <path class="displacy-arc" id="arrow-451ec6f4a0c84f5ca7f770751d33982b-0-13" stroke-width="2px" d="M420,527.0 C420,2.0 2500.0,2.0 2500.0,527.0" fill="none" stroke="currentColor"/>
    <text dy="1.25em" style="font-size: 0.8em; letter-spacing: 1px">
        <textPath xlink:href="#arrow-451ec6f4a0c84f5ca7f770751d33982b-0-13" class="displacy-label" startOffset="50%" side="left" fill="currentColor" text-anchor="middle">oc</textPath>
    </text>
    <path class="displacy-arrowhead" d="M2500.0,529.0 L2508.0,517.0 2492.0,517.0" fill="currentColor"/>
</g>
</svg></span>





    4




```python
# Installing DBpedia Spotlight library.
!pip install spacy-dbpedia-spotlight
```

    Requirement already satisfied: spacy-dbpedia-spotlight in /Users/kevin/opt/anaconda3/lib/python3.9/site-packages (0.2.5)
    Requirement already satisfied: loguru in /Users/kevin/opt/anaconda3/lib/python3.9/site-packages (from spacy-dbpedia-spotlight) (0.6.0)
    Requirement already satisfied: spacy>=3 in /Users/kevin/opt/anaconda3/lib/python3.9/site-packages (from spacy-dbpedia-spotlight) (3.4.1)
    Requirement already satisfied: pydantic!=1.8,!=1.8.1,<1.10.0,>=1.7.4 in /Users/kevin/opt/anaconda3/lib/python3.9/site-packages (from spacy>=3->spacy-dbpedia-spotlight) (1.9.2)
    Requirement already satisfied: jinja2 in /Users/kevin/opt/anaconda3/lib/python3.9/site-packages (from spacy>=3->spacy-dbpedia-spotlight) (2.11.3)
    Requirement already satisfied: spacy-legacy<3.1.0,>=3.0.9 in /Users/kevin/opt/anaconda3/lib/python3.9/site-packages (from spacy>=3->spacy-dbpedia-spotlight) (3.0.10)
    Requirement already satisfied: preshed<3.1.0,>=3.0.2 in /Users/kevin/opt/anaconda3/lib/python3.9/site-packages (from spacy>=3->spacy-dbpedia-spotlight) (3.0.7)
    Requirement already satisfied: cymem<2.1.0,>=2.0.2 in /Users/kevin/opt/anaconda3/lib/python3.9/site-packages (from spacy>=3->spacy-dbpedia-spotlight) (2.0.6)
    Requirement already satisfied: typer<0.5.0,>=0.3.0 in /Users/kevin/opt/anaconda3/lib/python3.9/site-packages (from spacy>=3->spacy-dbpedia-spotlight) (0.4.2)
    Requirement already satisfied: srsly<3.0.0,>=2.4.3 in /Users/kevin/opt/anaconda3/lib/python3.9/site-packages (from spacy>=3->spacy-dbpedia-spotlight) (2.4.4)
    Requirement already satisfied: wasabi<1.1.0,>=0.9.1 in /Users/kevin/opt/anaconda3/lib/python3.9/site-packages (from spacy>=3->spacy-dbpedia-spotlight) (0.10.1)
    Requirement already satisfied: setuptools in /Users/kevin/opt/anaconda3/lib/python3.9/site-packages (from spacy>=3->spacy-dbpedia-spotlight) (61.2.0)
    Requirement already satisfied: pathy>=0.3.5 in /Users/kevin/opt/anaconda3/lib/python3.9/site-packages (from spacy>=3->spacy-dbpedia-spotlight) (0.6.2)
    Requirement already satisfied: requests<3.0.0,>=2.13.0 in /Users/kevin/opt/anaconda3/lib/python3.9/site-packages (from spacy>=3->spacy-dbpedia-spotlight) (2.27.1)
    Requirement already satisfied: tqdm<5.0.0,>=4.38.0 in /Users/kevin/opt/anaconda3/lib/python3.9/site-packages (from spacy>=3->spacy-dbpedia-spotlight) (4.64.0)
    Requirement already satisfied: packaging>=20.0 in /Users/kevin/opt/anaconda3/lib/python3.9/site-packages (from spacy>=3->spacy-dbpedia-spotlight) (21.3)
    Requirement already satisfied: langcodes<4.0.0,>=3.2.0 in /Users/kevin/opt/anaconda3/lib/python3.9/site-packages (from spacy>=3->spacy-dbpedia-spotlight) (3.3.0)
    Requirement already satisfied: spacy-loggers<2.0.0,>=1.0.0 in /Users/kevin/opt/anaconda3/lib/python3.9/site-packages (from spacy>=3->spacy-dbpedia-spotlight) (1.0.3)
    Requirement already satisfied: numpy>=1.15.0 in /Users/kevin/opt/anaconda3/lib/python3.9/site-packages (from spacy>=3->spacy-dbpedia-spotlight) (1.21.5)
    Requirement already satisfied: murmurhash<1.1.0,>=0.28.0 in /Users/kevin/opt/anaconda3/lib/python3.9/site-packages (from spacy>=3->spacy-dbpedia-spotlight) (1.0.8)
    Requirement already satisfied: thinc<8.2.0,>=8.1.0 in /Users/kevin/opt/anaconda3/lib/python3.9/site-packages (from spacy>=3->spacy-dbpedia-spotlight) (8.1.3)
    Requirement already satisfied: catalogue<2.1.0,>=2.0.6 in /Users/kevin/opt/anaconda3/lib/python3.9/site-packages (from spacy>=3->spacy-dbpedia-spotlight) (2.0.8)
    Requirement already satisfied: pyparsing!=3.0.5,>=2.0.2 in /Users/kevin/opt/anaconda3/lib/python3.9/site-packages (from packaging>=20.0->spacy>=3->spacy-dbpedia-spotlight) (3.0.4)
    Requirement already satisfied: smart-open<6.0.0,>=5.2.1 in /Users/kevin/opt/anaconda3/lib/python3.9/site-packages (from pathy>=0.3.5->spacy>=3->spacy-dbpedia-spotlight) (5.2.1)
    Requirement already satisfied: typing-extensions>=3.7.4.3 in /Users/kevin/opt/anaconda3/lib/python3.9/site-packages (from pydantic!=1.8,!=1.8.1,<1.10.0,>=1.7.4->spacy>=3->spacy-dbpedia-spotlight) (4.1.1)
    Requirement already satisfied: certifi>=2017.4.17 in /Users/kevin/opt/anaconda3/lib/python3.9/site-packages (from requests<3.0.0,>=2.13.0->spacy>=3->spacy-dbpedia-spotlight) (2021.10.8)
    Requirement already satisfied: idna<4,>=2.5 in /Users/kevin/opt/anaconda3/lib/python3.9/site-packages (from requests<3.0.0,>=2.13.0->spacy>=3->spacy-dbpedia-spotlight) (3.3)
    Requirement already satisfied: urllib3<1.27,>=1.21.1 in /Users/kevin/opt/anaconda3/lib/python3.9/site-packages (from requests<3.0.0,>=2.13.0->spacy>=3->spacy-dbpedia-spotlight) (1.26.9)
    Requirement already satisfied: charset-normalizer~=2.0.0 in /Users/kevin/opt/anaconda3/lib/python3.9/site-packages (from requests<3.0.0,>=2.13.0->spacy>=3->spacy-dbpedia-spotlight) (2.0.4)
    Requirement already satisfied: confection<1.0.0,>=0.0.1 in /Users/kevin/opt/anaconda3/lib/python3.9/site-packages (from thinc<8.2.0,>=8.1.0->spacy>=3->spacy-dbpedia-spotlight) (0.0.3)
    Requirement already satisfied: blis<0.8.0,>=0.7.8 in /Users/kevin/opt/anaconda3/lib/python3.9/site-packages (from thinc<8.2.0,>=8.1.0->spacy>=3->spacy-dbpedia-spotlight) (0.7.8)
    Requirement already satisfied: click<9.0.0,>=7.1.1 in /Users/kevin/opt/anaconda3/lib/python3.9/site-packages (from typer<0.5.0,>=0.3.0->spacy>=3->spacy-dbpedia-spotlight) (8.0.4)
    Requirement already satisfied: MarkupSafe>=0.23 in /Users/kevin/opt/anaconda3/lib/python3.9/site-packages (from jinja2->spacy>=3->spacy-dbpedia-spotlight) (2.0.1)



```python
import spacy
nlp = spacy.load('de_core_news_sm')
nlp.add_pipe('dbpedia_spotlight', config = {'language_code': 'de'})

# Attempt at matching predicted entities with records to-be found in DBpedia.
doc = nlp("Karl-Heinz Quade ist von März 1944 bis August 1948 im Lager 150 in Grjasowez interniert.")
for ent in doc.ents:
    print(ent.text, ent.label_, ent.kb_id_)
```

    Grjasowez DBPEDIA_ENT http://de.dbpedia.org/resource/Grjasowez
    interniert DBPEDIA_ENT http://de.dbpedia.org/resource/Internierung



```python
# Requesting access to data associated with a match for "Grjasowez" from the DBpedia library.
import requests
data = requests.get("http://de.dbpedia.org/data/Grjasowez.json").json()
```


```python
# Accessing contents of data associated with a match for "Grjasowez" from the DBpedia library.
data
```




    {'http://de.dbpedia.org/resource/Obnora': {'http://dbpedia.org/ontology/sourceConfluence': [{'type': 'uri',
        'value': 'http://de.dbpedia.org/resource/Grjasowez'}]},
     'http://de.dbpedia.org/resource/Grjasowez': {'http://www.w3.org/1999/02/22-rdf-syntax-ns#type': [{'type': 'uri',
        'value': 'http://schema.org/Place'},
       {'type': 'uri',
        'value': 'http://www.w3.org/2003/01/geo/wgs84_pos#SpatialThing'},
       {'type': 'uri', 'value': 'http://www.w3.org/2002/07/owl#Thing'},
       {'type': 'uri', 'value': 'http://dbpedia.org/ontology/Settlement'},
       {'type': 'uri', 'value': 'http://dbpedia.org/ontology/PopulatedPlace'},
       {'type': 'uri', 'value': 'http://dbpedia.org/ontology/Location'},
       {'type': 'uri', 'value': 'http://www.wikidata.org/entity/Q486972'},
       {'type': 'uri', 'value': 'http://dbpedia.org/ontology/Place'}],
      'http://www.w3.org/2000/01/rdf-schema#label': [{'type': 'literal',
        'value': 'Grjasowez',
        'lang': 'de'}],
      'http://www.w3.org/2000/01/rdf-schema#comment': [{'type': 'literal',
        'value': 'Grjasowez (russisch Грязовец) ist eine kleine Kreisstadt mit 15.528 Einwohnern (Stand 14. Oktober 2010) in der Oblast Wologda im Norden des europäischen Teils Russlands.',
        'lang': 'de'}],
      'http://www.w3.org/2002/07/owl#sameAs': [{'type': 'uri',
        'value': 'http://dbpedia.org/resource/Gryazovets'},
       {'type': 'uri', 'value': 'http://fr.dbpedia.org/resource/Griazovets'},
       {'type': 'uri', 'value': 'http://www.wikidata.org/entity/Q134667'},
       {'type': 'uri', 'value': 'http://wikidata.dbpedia.org/resource/Q134667'},
       {'type': 'uri', 'value': 'http://de.dbpedia.org/resource/Grjasowez'},
       {'type': 'uri', 'value': 'http://rdf.freebase.com/ns/m.0gpnx7'},
       {'type': 'uri', 'value': 'http://ko.dbpedia.org/resource/그랴조베츠'},
       {'type': 'uri', 'value': 'http://it.dbpedia.org/resource/Grjazovec'},
       {'type': 'uri', 'value': 'http://pl.dbpedia.org/resource/Griazowiec'}],
      'http://www.georss.org/georss/point': [{'type': 'literal',
        'value': '58.88333333333333 40.25'}],
      'http://xmlns.com/foaf/0.1/name': [{'type': 'literal',
        'value': 'Grjasowez',
        'lang': 'de'},
       {'type': 'literal', 'value': 'Грязовец', 'lang': 'de'}],
      'http://www.w3.org/2003/01/geo/wgs84_pos#lat': [{'type': 'literal',
        'value': 58.88333511352539,
        'datatype': 'http://www.w3.org/2001/XMLSchema#float'}],
      'http://www.w3.org/2003/01/geo/wgs84_pos#long': [{'type': 'literal',
        'value': 40.25,
        'datatype': 'http://www.w3.org/2001/XMLSchema#float'}],
      'http://xmlns.com/foaf/0.1/depiction': [{'type': 'uri',
        'value': 'http://commons.wikimedia.org/wiki/Special:FilePath/Coat_of_Arms_of_Gryazovets_(Vologda_oblast)_(1781).png'}],
      'http://xmlns.com/foaf/0.1/homepage': [{'type': 'uri',
        'value': 'http://gradm.ru/'}],
      'http://purl.org/dc/terms/subject': [{'type': 'uri',
        'value': 'http://de.dbpedia.org/resource/Kategorie:Ort_in_der_Oblast_Wologda'}],
      'http://xmlns.com/foaf/0.1/isPrimaryTopicOf': [{'type': 'uri',
        'value': 'http://de.wikipedia.org/wiki/Grjasowez'}],
      'http://dbpedia.org/ontology/wikiPageID': [{'type': 'literal',
        'value': 2159725,
        'datatype': 'http://www.w3.org/2001/XMLSchema#integer'}],
      'http://dbpedia.org/ontology/wikiPageRevisionID': [{'type': 'literal',
        'value': 156713958,
        'datatype': 'http://www.w3.org/2001/XMLSchema#integer'}],
      'http://dbpedia.org/ontology/wikiPageExternalLink': [{'type': 'uri',
        'value': 'http://gryazovec.ru/'},
       {'type': 'uri',
        'value': 'http://russki-plen.ucoz.ru/_ld/0/93_lagergeschichte.pdf'},
       {'type': 'uri',
        'value': 'http://www.mojgorod.ru/vologod_obl/grjazovec/index.html'},
       {'type': 'uri', 'value': 'http://gradm.ru/'}],
      'http://de.dbpedia.org/property/artDesGebietes': [{'type': 'literal',
        'value': 'Rajon',
        'datatype': 'http://www.w3.org/1999/02/22-rdf-syntax-ns#langString'}],
      'http://de.dbpedia.org/property/ersteErwähnung': [{'type': 'literal',
        'value': 1538,
        'datatype': 'http://www.w3.org/2001/XMLSchema#integer'}],
      'http://de.dbpedia.org/property/gebiet': [{'type': 'literal',
        'value': 'Grjasowez',
        'datatype': 'http://www.w3.org/1999/02/22-rdf-syntax-ns#langString'}],
      'http://de.dbpedia.org/property/latDeg': [{'type': 'literal',
        'value': 58,
        'datatype': 'http://www.w3.org/2001/XMLSchema#integer'}],
      'http://de.dbpedia.org/property/latMin': [{'type': 'literal',
        'value': 53,
        'datatype': 'http://www.w3.org/2001/XMLSchema#integer'}],
      'http://de.dbpedia.org/property/latSec': [{'type': 'literal',
        'value': 0,
        'datatype': 'http://www.w3.org/2001/XMLSchema#integer'}],
      'http://de.dbpedia.org/property/lonDeg': [{'type': 'literal',
        'value': 40,
        'datatype': 'http://www.w3.org/2001/XMLSchema#integer'}],
      'http://de.dbpedia.org/property/lonMin': [{'type': 'literal',
        'value': 15,
        'datatype': 'http://www.w3.org/2001/XMLSchema#integer'}],
      'http://de.dbpedia.org/property/lonSec': [{'type': 'literal',
        'value': 0,
        'datatype': 'http://www.w3.org/2001/XMLSchema#integer'}],
      'http://de.dbpedia.org/property/oberhaupt': [{'type': 'literal',
        'value': 'Michail Rudakow',
        'datatype': 'http://www.w3.org/1999/02/22-rdf-syntax-ns#langString'}],
      'http://de.dbpedia.org/property/okato': [{'type': 'literal',
        'value': 19224501,
        'datatype': 'http://www.w3.org/2001/XMLSchema#integer'}],
      'http://de.dbpedia.org/property/status': [{'type': 'literal',
        'value': 'Stadt',
        'datatype': 'http://www.w3.org/1999/02/22-rdf-syntax-ns#langString'}],
      'http://de.dbpedia.org/property/statusSeit': [{'type': 'literal',
        'value': 1780,
        'datatype': 'http://www.w3.org/2001/XMLSchema#integer'}],
      'http://de.dbpedia.org/property/wappen': [{'type': 'literal',
        'value': 'Coat of Arms of Gryazovets  .png',
        'datatype': 'http://www.w3.org/1999/02/22-rdf-syntax-ns#langString'}],
      'http://www.w3.org/ns/prov#wasDerivedFrom': [{'type': 'uri',
        'value': 'http://de.wikipedia.org/wiki/Grjasowez?oldid=156713958'}],
      'http://dbpedia.org/ontology/PopulatedPlace/areaTotal': [{'type': 'literal',
        'value': '14.0',
        'datatype': 'http://dbpedia.org/datatype/squareKilometre'}],
      'http://dbpedia.org/ontology/abstract': [{'type': 'literal',
        'value': 'Grjasowez (russisch Грязовец) ist eine kleine Kreisstadt mit 15.528 Einwohnern (Stand 14. Oktober 2010) in der Oblast Wologda im Norden des europäischen Teils Russlands.',
        'lang': 'de'}],
      'http://dbpedia.org/ontology/areaCode': [{'type': 'literal',
        'value': '(+7) 81755'}],
      'http://dbpedia.org/ontology/areaTotal': [{'type': 'literal',
        'value': 14000000,
        'datatype': 'http://www.w3.org/2001/XMLSchema#double'}],
      'http://dbpedia.org/ontology/elevation': [{'type': 'literal',
        'value': 185,
        'datatype': 'http://www.w3.org/2001/XMLSchema#double'}],
      'http://dbpedia.org/ontology/leaderTitle': [{'type': 'literal',
        'value': 'Bürgermeister',
        'lang': 'de'}],
      'http://dbpedia.org/ontology/postalCode': [{'type': 'literal',
        'value': '162000–162002'}],
      'http://dbpedia.org/ontology/thumbnail': [{'type': 'uri',
        'value': 'http://commons.wikimedia.org/wiki/Special:FilePath/Coat_of_Arms_of_Gryazovets_(Vologda_oblast)_(1781).png?width=300'}]},
     'http://de.dbpedia.org/resource/Lew_Alexandrowitsch_Tschugajew': {'http://dbpedia.org/ontology/deathPlace': [{'type': 'uri',
        'value': 'http://de.dbpedia.org/resource/Grjasowez'}]},
     'http://de.dbpedia.org/resource/Gryazovets': {'http://dbpedia.org/ontology/wikiPageRedirects': [{'type': 'uri',
        'value': 'http://de.dbpedia.org/resource/Grjasowez'}]},
     'http://de.wikipedia.org/wiki/Grjasowez': {'http://xmlns.com/foaf/0.1/primaryTopic': [{'type': 'uri',
        'value': 'http://de.dbpedia.org/resource/Grjasowez'}]}}




```python
# Accessing contents of those keys utilized in data dictionary associated with a match for "Grjasowez" from the DBpedia library.
data.keys()
```




    dict_keys(['http://de.dbpedia.org/resource/Obnora', 'http://de.dbpedia.org/resource/Grjasowez', 'http://de.dbpedia.org/resource/Lew_Alexandrowitsch_Tschugajew', 'http://de.dbpedia.org/resource/Gryazovets', 'http://de.wikipedia.org/wiki/Grjasowez'])




```python
# Accessing latitude and longitutde of Grjasowez using DBpedia.
print(f"Latitude and Longitude of Grjasowez: {data['http://de.dbpedia.org/resource/Grjasowez']['http://www.georss.org/georss/point'][0]['value']}")
```

    Latitude and Longitude of Grjasowez: 58.88333333333333 40.25



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

    created:  karl-heinz_quade_diary.tsv


### Reflection
This Programming Historian lesson introduced me to some of the methods used by Digital Historians to scan series of text for terms of interest, with a particular focus on those terms associated with geographical locations. Gazeteers serve as directories containing a series of location names. Using such gazetteers as references, we are able to associate words from text as terms pertaining to the names of locations. This lesson introduced various gazetteers, however, the World Historical Gazetteer remains of special interest to me. This gazetteer serves as a database that has accumulated the names of geographical locations as they have changed throughout history. Using this database, digital historians have been offered the flexibility to analyze texts of historical periods at even greater degrees. Further, this lesson introduced me to Natural Language Processing tools that allow digital historians to analyze texts that have experienced transliteration, that is, the process of having been translated across languages and alphabets.

I see a great deal of potential within the computational methods this lesson introduced me to. Already, the World Historical Gazetteer contains an incredible amount of information about geographical regions across vast time periods. I feel excited to see all this great resource may offer in the future as researchers continue to add to this repository. In terms of what this resource may offer to humanist research, I believe that it will be of great aid in specifying the geographic and historical contexts for a piece of humanist evidence, which would surely offer provide researchers with a greater degree of freedom and accuracy when aiming to interpret such evidence in relation to the subject's sociocultural and historical contexts.
