---
layout: page
title: Final Project - GIS Mapping of Populated Regions Relevant to the Civil War
permalink: /final-project/
---
# GIS Mapping of Populated Regions Relevant to the Civil War in the View of Ulysses S. Grant's Personal Memoirs
Kevin Arellano Flores

CLS-0161 -- Introduction to Digital Humanities

Dr. Saxton

## Processing Data: Text Corpus, Ulysses S. Grant's Personal Memoirs


```python
# importing necessary python libraries and relevant methods
from sklearn.feature_extraction.text import TfidfVectorizer
from sklearn.feature_extraction.text import CountVectorizer
from sklearn.decomposition import NMF
from sklearn.decomposition import LatentDirichletAllocation

import pandas as pd
import bs4
import requests

import pprint
pp = pprint.PrettyPrinter(indent=4)
```


```python
# accessing text corpus, "Personal Memoirs of U.S. Grant" by Ulysses S. Grant through Project Gutenberg
response_text = requests.get('https://www.gutenberg.org/cache/epub/4367/pg4367-images.html').text
soup = bs4.BeautifulSoup(response_text, 'html.parser')
```


```python
# importing necessary python libraries
import collections
import re
```


```python
# producing default dictionary
paragraph = collections.defaultdict(str)

# populating dictionary with 'Paragraph' keys, and 'Text' values
i = 1
for p in soup.find_all('p')[254:862]:
    clean_title = 'Paragraph ' + str(i)
    paragraph[f'{clean_title}'] += p.text
    i += 1 
```


```python
# initializing dictionary variable
paragraph_dict = dict(paragraph)
print(paragraph_dict)
```

    {'Paragraph 1': 'My family, all this while, was at the East. It consisted now of\r\na wife and two children. I saw no chance of supporting them on the\r\nPacific coast out of my pay as an army officer. I concluded,\r\ntherefore, to resign, and in March applied for a leave of absence\r\nuntil the end of the July following, tendering my resignation to\r\ntake effect at the end of that time. I left the Pacific coast very\r\nmuch attached to it, and with the full expectation of making it my\r\nfuture home. That expectation and that hope remained uppermost in\r\nmy mind until the Lieutenant-Generalcy bill was introduced into\r\nCongress in the winter of 1863-4. The passage of that bill, and my\r\npromotion, blasted my last hope of ever becoming a citizen of the\r\nfurther West.', 'Paragraph 2': 'In the late summer of 1854 I rejoined my family, to find in it a\r\nson whom I had never seen, born while I was on the Isthmus of\r\nPanama. I was now to commence, at the age of thirty-two, a new\r\nstruggle for our support. My wife had a farm near St. Louis, to\r\nwhich we went, but I had no means to stock it. A house had to be\r\nbuilt also. I worked very hard, never losing a day because of bad\r\nweather, and accomplished the object in a moderate way. If nothing\r\nelse could be done I would load a cord of wood on a wagon and take\r\nit to the city for sale. I managed to keep along very well until\r\n1858, when I was attacked by fever and ague. I had suffered very\r\nseverely and for a long time from this disease, while a boy in\r\nOhio. It lasted now over a year, and, while it did not keep me in\r\nthe house, it did interfere greatly with the amount of work I was\r\nable to perform. In the fall of 1858 I sold out my stock, crops and\r\nfarming utensils at auction, and gave up farming.', 'Paragraph 3': "In the winter I established a partnership with Harry Boggs, a\r\ncousin of Mrs. Grant, in the real estate agency business. I spent\r\nthat winter at St. Louis myself, but did not take my family into\r\ntown until the spring. Our business might have become prosperous if\r\nI had been able to wait for it to grow. As it was, there was no\r\nmore than one person could attend to, and not enough to support two\r\nfamilies. While a citizen of St. Louis and engaged in the real\r\nestate agency business, I was a candidate for the office of county\r\nengineer, an office of respectability and emolument which would\r\nhave been very acceptable to me at that time. The incumbent was\r\nappointed by the county court, which consisted of five members. My\r\nopponent had the advantage of birth over me (he was a citizen by\r\nadoption) and carried off the prize. I now withdrew from the\r\nco-partnership with Boggs, and, in May, 1860, removed to Galena,\r\nIllinois, and took a clerkship in my father's store.", 'Paragraph 4': 'While a citizen of Missouri, my first opportunity for casting a\r\nvote at a Presidential election occurred. I had been in the army\r\nfrom before attaining my majority and had thought but little about\r\npolitics, although I was a Whig by education and a great admirer of\r\nMr. Clay. But the Whig party had ceased to exist before I had an\r\nopportunity of exercising the privilege of casting a ballot; the\r\nKnow-Nothing party had taken its place, but was on the wane; and\r\nthe Republican party was in a chaotic state and had not yet\r\nreceived a name. It had no existence in the Slave States except at\r\npoints on the borders next to Free States. In St. Louis City and\r\nCounty, what afterwards became the Republican party was known as\r\nthe Free-Soil Democracy, led by the Honorable Frank P. Blair. Most\r\nof my neighbors had known me as an officer of the army with Whig\r\nproclivities. They had been on the same side, and, on the death of\r\ntheir party, many had become Know-Nothings, or members of the\r\nAmerican party. There was a lodge near my new home, and I was\r\ninvited to join it. I accepted the invitation; was initiated;\r\nattended a meeting just one week later, and never went to another\r\nafterwards.', 'Paragraph 5': 'I have no apologies to make for having been one week a member of\r\nthe American party; for I still think native-born citizens of the\r\nUnited States should have as much protection, as many privileges in\r\ntheir native country, as those who voluntarily select it for a\r\nhome. But all secret, oath-bound political parties are dangerous to\r\nany nation, no matter how pure or how patriotic the motives and\r\nprinciples which first bring them together. No political party can\r\nor ought to exist when one of its corner-stones is opposition to\r\nfreedom of thought and to the right to worship God "according to\r\nthe dictate of one\'s own conscience," or according to the creed of\r\nany religious denomination whatever. Nevertheless, if a sect sets\r\nup its laws as binding above the State laws, wherever the two come\r\nin conflict this claim must be resisted and suppressed at whatever\r\ncost.', 'Paragraph 6': 'Up to the Mexican war there were a few out and out\r\nabolitionists, men who carried their hostility to slavery into all\r\nelections, from those for a justice of the peace up to the\r\nPresidency of the United States. They were noisy but not numerous.\r\nBut the great majority of people at the North, where slavery did\r\nnot exist, were opposed to the institution, and looked upon its\r\nexistence in any part of the country as unfortunate. They did not\r\nhold the States where slavery existed responsible for it; and\r\nbelieved that protection should be given to the right of property\r\nin slaves until some satisfactory way could be reached to be rid of\r\nthe institution. Opposition to slavery was not a creed of either\r\npolitical party. In some sections more anti-slavery men belonged to\r\nthe Democratic party, and in others to the Whigs. But with the\r\ninauguration of the Mexican war, in fact with the annexation of\r\nTexas, "the inevitable conflict" commenced.', 'Paragraph 7': 'As the time for the Presidential election of 1856—the\r\nfirst at which I had the opportunity of voting—approached,\r\nparty feeling began to run high. The Republican party was regarded\r\nin the South and the border States not only as opposed to the\r\nextension of slavery, but as favoring the compulsory abolition of\r\nthe institution without compensation to the owners. The most\r\nhorrible visions seemed to present themselves to the minds of\r\npeople who, one would suppose, ought to have known better. Many\r\neducated and, otherwise, sensible persons appeared to believe that\r\nemancipation meant social equality. Treason to the Government was\r\nopenly advocated and was not rebuked. It was evident to my mind\r\nthat the election of a Republican President in 1856 meant the\r\nsecession of all the Slave States, and rebellion. Under these\r\ncircumstances I preferred the success of a candidate whose election\r\nwould prevent or postpone secession, to seeing the country plunged\r\ninto a war the end of which no man could foretell. With a Democrat\r\nelected by the unanimous vote of the Slave States, there could be\r\nno pretext for secession for four years. I very much hoped that the\r\npassions of the people would subside in that time, and the\r\ncatastrophe be averted altogether; if it was not, I believed the\r\ncountry would be better prepared to receive the shock and to resist\r\nit. I therefore voted for James Buchanan for President. Four years\r\nlater the Republican party was successful in electing its candidate\r\nto the Presidency. The civilized world has learned the consequence.\r\nFour millions of human beings held as chattels have been liberated;\r\nthe ballot has been given to them; the free schools of the country\r\nhave been opened to their children. The nation still lives, and the\r\npeople are just as free to avoid social intimacy with the blacks as\r\never they were, or as they are with white people.', 'Paragraph 8': "While living in Galena I was nominally only a clerk supporting\r\nmyself and family on a stipulated salary. In reality my position\r\nwas different. My father had never lived in Galena himself, but had\r\nestablished my two brothers there, the one next younger than myself\r\nin charge of the business, assisted by the youngest. When I went\r\nthere it was my father's intention to give up all connection with\r\nthe business himself, and to establish his three sons in it: but\r\nthe brother who had really built up the business was sinking with\r\nconsumption, and it was not thought best to make any change while\r\nhe was in this condition. He lived until September, 1861, when he\r\nsuccumbed to that insidious disease which always flatters its\r\nvictims into the belief that they are growing better up to the\r\nclose of life. A more honorable man never transacted business. In\r\nSeptember, 1861, I was engaged in an employment which required all\r\nmy attention elsewhere.", 'Paragraph 9': 'During the eleven months that I lived in Galena prior to the\r\nfirst call for volunteers, I had been strictly attentive to my\r\nbusiness, and had made but few acquaintances other than customers\r\nand people engaged in the same line with myself. When the election\r\ntook place in November, 1860, I had not been a resident of Illinois\r\nlong enough to gain citizenship and could not, therefore, vote. I\r\nwas really glad of this at the time, for my pledges would have\r\ncompelled me to vote for Stephen A. Douglas, who had no possible\r\nchance of election. The contest was really between Mr. Breckinridge\r\nand Mr. Lincoln; between minority rule and rule by the majority. I\r\nwanted, as between these candidates, to see Mr. Lincoln elected.\r\nExcitement ran high during the canvass, and torch-light processions\r\nenlivened the scene in the generally quiet streets of Galena many\r\nnights during the campaign. I did not parade with either party, but\r\noccasionally met with the "wide awakes"—Republicans—in\r\ntheir rooms, and superintended their drill. It was evident, from\r\nthe time of the Chicago nomination to the close of the canvass,\r\nthat the election of the Republican candidate would be the signal\r\nfor some of the Southern States to secede. I still had hopes that\r\nthe four years which had elapsed since the first nomination of a\r\nPresidential candidate by a party distinctly opposed to slavery\r\nextension, had given time for the extreme pro-slavery sentiment to\r\ncool down; for the Southerners to think well before they took the\r\nawful leap which they had so vehemently threatened. But I was\r\nmistaken.', 'Paragraph 10': 'The Republican candidate was elected, and solid substantial\r\npeople of the North-west, and I presume the same order of people\r\nthroughout the entire North, felt very serious, but determined,\r\nafter this event. It was very much discussed whether the South\r\nwould carry out its threat to secede and set up a separate\r\ngovernment, the corner-stone of which should be, protection to the\r\n"Divine" institution of slavery. For there were people who believed\r\nin the "divinity" of human slavery, as there are now people who\r\nbelieve Mormonism and Polygamy to be ordained by the Most High. We\r\nforgive them for entertaining such notions, but forbid their\r\npractice. It was generally believed that there would be a flurry;\r\nthat some of the extreme Southern States would go so far as to pass\r\nordinances of secession. But the common impression was that this\r\nstep was so plainly suicidal for the South, that the movement would\r\nnot spread over much of the territory and would not last long.', 'Paragraph 11': 'Doubtless the founders of our government, the majority of them\r\nat least, regarded the confederation of the colonies as an\r\nexperiment. Each colony considered itself a separate government;\r\nthat the confederation was for mutual protection against a foreign\r\nfoe, and the prevention of strife and war among themselves. If\r\nthere had been a desire on the part of any single State to withdraw\r\nfrom the compact at any time while the number of States was limited\r\nto the original thirteen, I do not suppose there would have been\r\nany to contest the right, no matter how much the determination\r\nmight have been regretted. The problem changed on the ratification\r\nof the Constitution by all the colonies; it changed still more when\r\namendments were added; and if the right of any one State to\r\nwithdraw continued to exist at all after the ratification of the\r\nConstitution, it certainly ceased on the formation of new States,\r\nat least so far as the new States themselves were concerned. It was\r\nnever possessed at all by Florida or the States west of the\r\nMississippi, all of which were purchased by the treasury of the\r\nentire nation. Texas and the territory brought into the Union in\r\nconsequence of annexation, were purchased with both blood and\r\ntreasure; and Texas, with a domain greater than that of any\r\nEuropean state except Russia, was permitted to retain as state\r\nproperty all the public lands within its borders. It would have\r\nbeen ingratitude and injustice of the most flagrant sort for this\r\nState to withdraw from the Union after all that had been spent and\r\ndone to introduce her; yet, if separation had actually occurred,\r\nTexas must necessarily have gone with the South, both on account of\r\nher institutions and her geographical position. Secession was\r\nillogical as well as impracticable; it was revolution.', 'Paragraph 12': 'Now, the right of revolution is an inherent one. When people are\r\noppressed by their government, it is a natural right they enjoy to\r\nrelieve themselves of the oppression, if they are strong enough,\r\neither by withdrawal from it, or by overthrowing it and\r\nsubstituting a government more acceptable. But any people or part\r\nof a people who resort to this remedy, stake their lives, their\r\nproperty, and every claim for protection given by\r\ncitizenship—on the issue. Victory, or the conditions imposed\r\nby the conqueror—must be the result.', 'Paragraph 13': 'In the case of the war between the States it would have been the\r\nexact truth if the South had said,—"We do not want to live\r\nwith you Northern people any longer; we know our institution of\r\nslavery is obnoxious to you, and, as you are growing numerically\r\nstronger than we, it may at some time in the future be endangered.\r\nSo long as you permitted us to control the government, and with the\r\naid of a few friends at the North to enact laws constituting your\r\nsection a guard against the escape of our property, we were willing\r\nto live with you. You have been submissive to our rule heretofore;\r\nbut it looks now as if you did not intend to continue so, and we\r\nwill remain in the Union no longer." Instead of this the seceding\r\nStates cried lustily,—"Let us alone; you have no\r\nconstitutional power to interfere with us." Newspapers and people\r\nat the North reiterated the cry. Individuals might ignore the\r\nconstitution; but the Nation itself must not only obey it, but must\r\nenforce the strictest construction of that instrument; the\r\nconstruction put upon it by the Southerners themselves. The fact is\r\nthe constitution did not apply to any such contingency as the one\r\nexisting from 1861 to 1865. Its framers never dreamed of such a\r\ncontingency occurring. If they had foreseen it, the probabilities\r\nare they would have sanctioned the right of a State or States to\r\nwithdraw rather than that there should be war between brothers.', 'Paragraph 14': 'The framers were wise in their generation and wanted to do the\r\nvery best possible to secure their own liberty and independence,\r\nand that also of their descendants to the latest days. It is\r\npreposterous to suppose that the people of one generation can lay\r\ndown the best and only rules of government for all who are to come\r\nafter them, and under unforeseen contingencies. At the time of the\r\nframing of our constitution the only physical forces that had been\r\nsubdued and made to serve man and do his labor, were the currents\r\nin the streams and in the air we breathe. Rude machinery, propelled\r\nby water power, had been invented; sails to propel ships upon the\r\nwaters had been set to catch the passing breeze—but the\r\napplication of stream to propel vessels against both wind and\r\ncurrent, and machinery to do all manner of work had not been\r\nthought of. The instantaneous transmission of messages around the\r\nworld by means of electricity would probably at that day have been\r\nattributed to witchcraft or a league with the Devil. Immaterial\r\ncircumstances had changed as greatly as material ones. We could not\r\nand ought not to be rigidly bound by the rules laid down under\r\ncircumstances so different for emergencies so utterly\r\nunanticipated. The fathers themselves would have been the first to\r\ndeclare that their prerogatives were not irrevocable. They would\r\nsurely have resisted secession could they have lived to see the\r\nshape it assumed.', 'Paragraph 15': 'I travelled through the Northwest considerably during the winter\r\nof 1860-1. We had customers in all the little towns in south-west\r\nWisconsin, south-east Minnesota and north-east Iowa. These\r\ngenerally knew I had been a captain in the regular army and had\r\nserved through the Mexican war. Consequently wherever I stopped at\r\nnight, some of the people would come to the public-house where I\r\nwas, and sit till a late hour discussing the probabilities of the\r\nfuture. My own views at that time were like those officially\r\nexpressed by Mr. Seward at a later day, that "the war would be over\r\nin ninety days." I continued to entertain these views until after\r\nthe battle of Shiloh. I believe now that there would have been no\r\nmore battles at the West after the capture of Fort Donelson if all\r\nthe troops in that region had been under a single commander who\r\nwould have followed up that victory.', 'Paragraph 16': "There is little doubt in my mind now that the prevailing\r\nsentiment of the South would have been opposed to secession in 1860\r\nand 1861, if there had been a fair and calm expression of opinion,\r\nunbiased by threats, and if the ballot of one legal voter had\r\ncounted for as much as that of any other. But there was no calm\r\ndiscussion of the question. Demagogues who were too old to enter\r\nthe army if there should be a war, others who entertained so high\r\nan opinion of their own ability that they did not believe they\r\ncould be spared from the direction of the affairs of state in such\r\nan event, declaimed vehemently and unceasingly against the North;\r\nagainst its aggressions upon the South; its interference with\r\nSouthern rights, etc., etc. They denounced the Northerners as\r\ncowards, poltroons, negro-worshippers; claimed that one Southern\r\nman was equal to five Northern men in battle; that if the South\r\nwould stand up for its rights the North would back down. Mr.\r\nJefferson Davis said in a speech, delivered at La Grange,\r\nMississippi, before the secession of that State, that he would\r\nagree to drink all the blood spilled south of Mason and Dixon's\r\nline if there should be a war. The young men who would have the\r\nfighting to do in case of war, believed all these statements, both\r\nin regard to the aggressiveness of the North and its cowardice.\r\nThey, too, cried out for a separation from such people. The great\r\nbulk of the legal voters of the South were men who owned no slaves;\r\ntheir homes were generally in the hills and poor country; their\r\nfacilities for educating their children, even up to the point of\r\nreading and writing, were very limited; their interest in the\r\ncontest was very meagre—what there was, if they had been\r\ncapable of seeing it, was with the North; they too needed\r\nemancipation. Under the old regime they were looked down upon by\r\nthose who controlled all the affairs in the interest of\r\nslave-owners, as poor white trash who were allowed the ballot so\r\nlong as they cast it according to direction.", 'Paragraph 17': 'I am aware that this last statement may be disputed and\r\nindividual testimony perhaps adduced to show that in ante-bellum\r\ndays the ballot was as untrammelled in the south as in any section\r\nof the country; but in the face of any such contradiction I\r\nreassert the statement. The shot-gun was not resorted to. Masked\r\nmen did not ride over the country at night intimidating voters; but\r\nthere was a firm feeling that a class existed in every State with a\r\nsort of divine right to control public affairs. If they could not\r\nget this control by one means they must by another. The end\r\njustified the means. The coercion, if mild, was complete.', 'Paragraph 18': 'There were two political parties, it is true, in all the States,\r\nboth strong in numbers and respectability, but both equally loyal\r\nto the institution which stood paramount in Southern eyes to all\r\nother institutions in state or nation. The slave-owners were the\r\nminority, but governed both parties. Had politics ever divided the\r\nslave-holders and the non-slave-holders, the majority would have\r\nbeen obliged to yield, or internecine war would have been the\r\nconsequence. I do not know that the Southern people were to blame\r\nfor this condition of affairs. There was a time when slavery was\r\nnot profitable, and the discussion of the merits of the institution\r\nwas confined almost exclusively to the territory where it existed.\r\nThe States of Virginia and Kentucky came near abolishing slavery by\r\ntheir own acts, one State defeating the measure by a tie vote and\r\nthe other only lacking one. But when the institution became\r\nprofitable, all talk of its abolition ceased where it existed; and\r\nnaturally, as human nature is constituted, arguments were adduced\r\nin its support. The cotton-gin probably had much to do with the\r\njustification of slavery.', 'Paragraph 19': 'The winter of 1860-1 will be remembered by middle-aged people of\r\nto-day as one of great excitement. South Carolina promptly seceded\r\nafter the result of the Presidential election was known. Other\r\nSouthern States proposed to follow. In some of them the Union\r\nsentiment was so strong that it had to be suppressed by force.\r\nMaryland, Delaware, Kentucky and Missouri, all Slave States, failed\r\nto pass ordinances of secession; but they were all represented in\r\nthe so-called congress of the so-called Confederate States. The\r\nGovernor and Lieutenant-Governor of Missouri, in 1861, Jackson and\r\nReynolds, were both supporters of the rebellion and took refuge\r\nwith the enemy. The governor soon died, and the lieutenant-governor\r\nassumed his office; issued proclamations as governor of the State;\r\nwas recognized as such by the Confederate Government, and continued\r\nhis pretensions until the collapse of the rebellion. The South\r\nclaimed the sovereignty of States, but claimed the right to coerce\r\ninto their confederation such States as they wanted, that is, all\r\nthe States where slavery existed. They did not seem to think this\r\ncourse inconsistent. The fact is, the Southern slave-owners\r\nbelieved that, in some way, the ownership of slaves conferred a\r\nsort of patent of nobility—a right to govern independent of\r\nthe interest or wishes of those who did not hold such property.\r\nThey convinced themselves, first, of the divine origin of the\r\ninstitution and, next, that that particular institution was not\r\nsafe in the hands of any body of legislators but themselves.', 'Paragraph 20': 'Meanwhile the Administration of President Buchanan looked\r\nhelplessly on and proclaimed that the general government had no\r\npower to interfere; that the Nation had no power to save its own\r\nlife. Mr. Buchanan had in his cabinet two members at least, who\r\nwere as earnest—to use a mild term—in the cause of\r\nsecession as Mr. Davis or any Southern statesman. One of them,\r\nFloyd, the Secretary of War, scattered the army so that much of it\r\ncould be captured when hostilities should commence, and distributed\r\nthe cannon and small arms from Northern arsenals throughout the\r\nSouth so as to be on hand when treason wanted them. The navy was\r\nscattered in like manner. The President did not prevent his cabinet\r\npreparing for war upon their government, either by destroying its\r\nresources or storing them in the South until a de facto government\r\nwas established with Jefferson Davis as its President, and\r\nMontgomery, Alabama, as the Capital. The secessionists had then to\r\nleave the cabinet. In their own estimation they were aliens in the\r\ncountry which had given them birth. Loyal men were put into their\r\nplaces. Treason in the executive branch of the government was\r\nestopped. But the harm had already been done. The stable door was\r\nlocked after the horse had been stolen.', 'Paragraph 21': 'During all of the trying winter of 1860-1, when the Southerners\r\nwere so defiant that they would not allow within their borders the\r\nexpression of a sentiment hostile to their views, it was a brave\r\nman indeed who could stand up and proclaim his loyalty to the\r\nUnion. On the other hand men at the North—prominent\r\nmen—proclaimed that the government had no power to coerce the\r\nSouth into submission to the laws of the land; that if the North\r\nundertook to raise armies to go south, these armies would have to\r\nmarch over the dead bodies of the speakers. A portion of the press\r\nof the North was constantly proclaiming similar views. When the\r\ntime arrived for the President-elect to go to the capital of the\r\nNation to be sworn into office, it was deemed unsafe for him to\r\ntravel, not only as a President-elect, but as any private citizen\r\nshould be allowed to do. Instead of going in a special car,\r\nreceiving the good wishes of his constituents at all the stations\r\nalong the road, he was obliged to stop on the way and to be\r\nsmuggled into the capital. He disappeared from public view on his\r\njourney, and the next the country knew, his arrival was announced\r\nat the capital. There is little doubt that he would have been\r\nassassinated if he had attempted to travel openly throughout his\r\njourney.', 'Paragraph 22': 'The 4th of March, 1861, came, and Abraham Lincoln was sworn to\r\nmaintain the Union against all its enemies. The secession of one\r\nState after another followed, until eleven had gone out. On the\r\n11th of April Fort Sumter, a National fort in the harbor of\r\nCharleston, South Carolina, was fired upon by the Southerners and a\r\nfew days after was captured. The Confederates proclaimed themselves\r\naliens, and thereby debarred themselves of all right to claim\r\nprotection under the Constitution of the United States. We did not\r\nadmit the fact that they were aliens, but all the same, they\r\ndebarred themselves of the right to expect better treatment than\r\npeople of any other foreign state who make war upon an independent\r\nnation. Upon the firing on Sumter President Lincoln issued his\r\nfirst call for troops and soon after a proclamation convening\r\nCongress in extra session. The call was for 75,000 volunteers for\r\nninety days\' service. If the shot fired at Fort Sumter "was heard\r\naround the world," the call of the President for 75,000 men was\r\nheard throughout the Northern States. There was not a state in the\r\nNorth of a million of inhabitants that would not have furnished the\r\nentire number faster than arms could have been supplied to them, if\r\nit had been necessary.', 'Paragraph 23': 'As soon as the news of the call for volunteers reached Galena,\r\nposters were stuck up calling for a meeting of the citizens at the\r\ncourt-house in the evening. Business ceased entirely; all was\r\nexcitement; for a time there were no party distinctions; all were\r\nUnion men, determined to avenge the insult to the national flag. In\r\nthe evening the court-house was packed. Although a comparative\r\nstranger I was called upon to preside; the sole reason, possibly,\r\nwas that I had been in the army and had seen service. With much\r\nembarrassment and some prompting I made out to announce the object\r\nof the meeting. Speeches were in order, but it is doubtful whether\r\nit would have been safe just then to make other than patriotic\r\nones. There was probably no one in the house, however, who felt\r\nlike making any other. The two principal speeches were by B. B.\r\nHoward, the post-master and a Breckinridge Democrat at the November\r\nelection the fall before, and John A. Rawlins, an elector on the\r\nDouglas ticket. E. B. Washburne, with whom I was not acquainted at\r\nthat time, came in after the meeting had been organized, and\r\nexpressed, I understood afterwards, a little surprise that Galena\r\ncould not furnish a presiding officer for such an occasion without\r\ntaking a stranger. He came forward and was introduced, and made a\r\nspeech appealing to the patriotism of the meeting.', 'Paragraph 24': 'After the speaking was over volunteers were called for to form a\r\ncompany. The quota of Illinois had been fixed at six regiments; and\r\nit was supposed that one company would be as much as would be\r\naccepted from Galena. The company was raised and the officers and\r\nnon-commissioned officers elected before the meeting adjourned. I\r\ndeclined the captaincy before the balloting, but announced that I\r\nwould aid the company in every way I could and would be found in\r\nthe service in some position if there should be a war. I never went\r\ninto our leather store after that meeting, to put up a package or\r\ndo other business.', 'Paragraph 25': 'The ladies of Galena were quite as patriotic as the men. They\r\ncould not enlist, but they conceived the idea of sending their\r\nfirst company to the field uniformed. They came to me to get a\r\ndescription of the United States uniform for infantry; subscribed\r\nand bought the material; procured tailors to cut out the garments,\r\nand the ladies made them up. In a few days the company was in\r\nuniform and ready to report at the State capital for assignment.\r\nThe men all turned out the morning after their enlistment, and I\r\ntook charge, divided them into squads and superintended their\r\ndrill. When they were ready to go to Springfield I went with them\r\nand remained there until they were assigned to a regiment.', 'Paragraph 26': 'There were so many more volunteers than had been called for that\r\nthe question whom to accept was quite embarrassing to the governor,\r\nRichard Yates. The legislature was in session at the time, however,\r\nand came to his relief. A law was enacted authorizing the governor\r\nto accept the services of ten additional regiments, one from each\r\ncongressional district, for one month, to be paid by the State, but\r\npledged to go into the service of the United States if there should\r\nbe a further call during their term. Even with this relief the\r\ngovernor was still very much embarrassed. Before the war was over\r\nhe was like the President when he was taken with the varioloid: "at\r\nlast he had something he could give to all who wanted it."', 'Paragraph 27': 'In time the Galena company was mustered into the United States\r\nservice, forming a part of the 11th Illinois volunteer infantry. My\r\nduties, I thought, had ended at Springfield, and I was prepared to\r\nstart home by the evening train, leaving at nine o\'clock. Up to\r\nthat time I do not think I had been introduced to Governor Yates,\r\nor had ever spoken to him. I knew him by sight, however, because he\r\nwas living at the same hotel and I often saw him at table. The\r\nevening I was to quit the capital I left the supper room before the\r\ngovernor and was standing at the front door when he came out. He\r\nspoke to me, calling me by my old army title "Captain," and said he\r\nunderstood that I was about leaving the city. I answered that I\r\nwas. He said he would be glad if I would remain over-night and call\r\nat the Executive office the next morning. I complied with his\r\nrequest, and was asked to go into the Adjutant-General\'s office and\r\nrender such assistance as I could, the governor saying that my army\r\nexperience would be of great service there. I accepted the\r\nproposition.', 'Paragraph 28': 'My old army experience I found indeed of very great service. I\r\nwas no clerk, nor had I any capacity to become one. The only place\r\nI ever found in my life to put a paper so as to find it again was\r\neither a side coat-pocket or the hands of a clerk or secretary more\r\ncareful than myself. But I had been quartermaster, commissary and\r\nadjutant in the field. The army forms were familiar to me and I\r\ncould direct how they should be made out. There was a clerk in the\r\noffice of the Adjutant-General who supplied my deficiencies. The\r\nease with which the State of Illinois settled its accounts with the\r\ngovernment at the close of the war is evidence of the efficiency of\r\nMr. Loomis as an accountant on a large scale. He remained in the\r\noffice until that time.', 'Paragraph 29': 'As I have stated, the legislature authorized the governor to\r\naccept the services of ten additional regiments. I had charge of\r\nmustering these regiments into the State service. They were\r\nassembled at the most convenient railroad centres in their\r\nrespective congressional districts. I detailed officers to muster\r\nin a portion of them, but mustered three in the southern part of\r\nthe State myself. One of these was to assemble at Belleville, some\r\neighteen miles south-east of St. Louis. When I got there I found\r\nthat only one or two companies had arrived. There was no\r\nprobability of the regiment coming together under five days. This\r\ngave me a few idle days which I concluded to spend in St.\r\nLouis.', 'Paragraph 30': 'There was a considerable force of State militia at Camp Jackson,\r\non the outskirts of St. Louis, at the time. There is but little\r\ndoubt that it was the design of Governor Claiborn Jackson to have\r\nthese troops ready to seize the United States arsenal and the city\r\nof St. Louis. Why they did not do so I do not know. There was but a\r\nsmall garrison, two companies I think, under Captain N. Lyon at the\r\narsenal, and but for the timely services of the Hon. F. P. Blair, I\r\nhave little doubt that St. Louis would have gone into rebel hands,\r\nand with it the arsenal with all its arms and ammunition.', 'Paragraph 31': "Blair was a leader among the Union men of St. Louis in 1861.\r\nThere was no State government in Missouri at the time that would\r\nsanction the raising of troops or commissioned officers to protect\r\nUnited States property, but Blair had probably procured some form\r\nof authority from the President to raise troops in Missouri and to\r\nmuster them into the service of the United States. At all events,\r\nhe did raise a regiment and took command himself as Colonel. With\r\nthis force he reported to Captain Lyon and placed himself and\r\nregiment under his orders. It was whispered that Lyon thus\r\nreinforced intended to break up Camp Jackson and capture the\r\nmilitia. I went down to the arsenal in the morning to see the\r\ntroops start out. I had known Lyon for two years at West Point and\r\nin the old army afterwards. Blair I knew very well by sight. I had\r\nheard him speak in the canvass of 1858, possibly several times, but\r\nI had never spoken to him. As the troops marched out of the\r\nenclosure around the arsenal, Blair was on his horse outside\r\nforming them into line preparatory to their march. I introduced\r\nmyself to him and had a few moments' conversation and expressed my\r\nsympathy with his purpose. This was my first personal acquaintance\r\nwith the Honorable—afterwards Major-General F. P. Blair. Camp\r\nJackson surrendered without a fight and the garrison was marched\r\ndown to the arsenal as prisoners of war.", 'Paragraph 32': 'Up to this time the enemies of the government in St. Louis had\r\nbeen bold and defiant, while Union men were quiet but determined.\r\nThe enemies had their head-quarters in a central and public\r\nposition on Pine Street, near Fifth—from which the rebel flag\r\nwas flaunted boldly. The Union men had a place of meeting somewhere\r\nin the city, I did not know where, and I doubt whether they dared\r\nto enrage the enemies of the government by placing the national\r\nflag outside their head-quarters. As soon as the news of the\r\ncapture of Camp Jackson reached the city the condition of affairs\r\nwas changed. Union men became rampant, aggressive, and, if you\r\nwill, intolerant. They proclaimed their sentiments boldly, and were\r\nimpatient at anything like disrespect for the Union. The\r\nsecessionists became quiet but were filled with suppressed rage.\r\nThey had been playing the bully. The Union men ordered the rebel\r\nflag taken down from the building on Pine Street. The command was\r\ngiven in tones of authority and it was taken down, never to be\r\nraised again in St. Louis.', 'Paragraph 33': 'I witnessed the scene. I had heard of the surrender of the camp\r\nand that the garrison was on its way to the arsenal. I had seen the\r\ntroops start out in the morning and had wished them success. I now\r\ndetermined to go to the arsenal and await their arrival and\r\ncongratulate them. I stepped on a car standing at the corner of 4th\r\nand Pine streets, and saw a crowd of people standing quietly in\r\nfront of the head-quarters, who were there for the purpose of\r\nhauling down the flag. There were squads of other people at\r\nintervals down the street. They too were quiet but filled with\r\nsuppressed rage, and muttered their resentment at the insult to,\r\nwhat they called, "their" flag. Before the car I was in had\r\nstarted, a dapper little fellow—he would be called a dude at\r\nthis day—stepped in. He was in a great state of excitement\r\nand used adjectives freely to express his contempt for the Union\r\nand for those who had just perpetrated such an outrage upon the\r\nrights of a free people. There was only one other passenger in the\r\ncar besides myself when this young man entered. He evidently\r\nexpected to find nothing but sympathy when he got away from the\r\n"mud sills" engaged in compelling a "free people" to pull down a\r\nflag they adored. He turned to me saying: "Things have come to a\r\n—— pretty pass when a free people can\'t choose their\r\nown flag. Where I came from if a man dares to say a word in favor\r\nof the Union we hang him to a limb of the first tree we come to." I\r\nreplied that "after all we were not so intolerant in St. Louis as\r\nwe might be; I had not seen a single rebel hung yet, nor heard of\r\none; there were plenty of them who ought to be, however." The young\r\nman subsided. He was so crestfallen that I believe if I had ordered\r\nhim to leave the car he would have gone quietly out, saying to\r\nhimself: "More Yankee oppression."', 'Paragraph 34': 'By nightfall the late defenders of Camp Jackson were all within\r\nthe walls of the St. Louis arsenal, prisoners of war. The next day\r\nI left St. Louis for Mattoon, Illinois, where I was to muster in\r\nthe regiment from that congressional district. This was the 21st\r\nIllinois infantry, the regiment of which I subsequently became\r\ncolonel. I mustered one regiment afterwards, when my services for\r\nthe State were about closed.', 'Paragraph 35': 'Brigadier-General John Pope was stationed at Springfield, as\r\nUnited States mustering officer, all the time I was in the State\r\nservice. He was a native of Illinois and well acquainted with most\r\nof the prominent men in the State. I was a carpet-bagger and knew\r\nbut few of them. While I was on duty at Springfield the senators,\r\nrepresentatives in Congress, ax-governors and the State legislators\r\nwere nearly all at the State capital. The only acquaintance I made\r\namong them was with the governor, whom I was serving, and, by\r\nchance, with Senator S. A. Douglas. The only members of Congress I\r\nknew were Washburne and Philip Foulk. With the former, though he\r\nrepresented my district and we were citizens of the same town, I\r\nonly became acquainted at the meeting when the first company of\r\nGalena volunteers was raised. Foulk I had known in St. Louis when I\r\nwas a citizen of that city. I had been three years at West Point\r\nwith Pope and had served with him a short time during the Mexican\r\nwar, under General Taylor. I saw a good deal of him during my\r\nservice with the State. On one occasion he said to me that I ought\r\nto go into the United States service. I told him I intended to do\r\nso if there was a war. He spoke of his acquaintance with the public\r\nmen of the State, and said he could get them to recommend me for a\r\nposition and that he would do all he could for me. I declined to\r\nreceive endorsement for permission to fight for my country.', 'Paragraph 36': 'Going home for a day or two soon after this conversation with\r\nGeneral Pope, I wrote from Galena the following letter to the\r\nAdjutant-General of the Army.', 'Paragraph 37': 'GALENA, ILLINOIS, May 24, 1861.', 'Paragraph 38': 'COL. L. THOMAS Adjt. Gen. U. S. A., Washington, D. C.', 'Paragraph 39': 'SIR:—Having served for fifteen years in the regular army,\r\nincluding four years at West Point, and feeling it the duty of\r\nevery one who has been educated at the Government expense to offer\r\ntheir services for the support of that Government, I have the\r\nhonor, very respectfully, to tender my services, until the close of\r\nthe war, in such capacity as may be offered. I would say, in view\r\nof my present age and length of service, I feel myself competent to\r\ncommand a regiment, if the President, in his judgment, should see\r\nfit to intrust one to me.', 'Paragraph 40': 'Since the first call of the President I have been serving on the\r\nstaff of the Governor of this State, rendering such aid as I could\r\nin the organization of our State militia, and am still engaged in\r\nthat capacity. A letter addressed to me at Springfield, Illinois,\r\nwill reach me.', 'Paragraph 41': 'I am very respectfully, Your obt. svt., U. S. GRANT.', 'Paragraph 42': 'This letter failed to elicit an answer from the Adjutant-General\r\nof the Army. I presume it was hardly read by him, and certainly it\r\ncould not have been submitted to higher authority. Subsequent to\r\nthe war General Badeau having heard of this letter applied to the\r\nWar Department for a copy of it. The letter could not be found and\r\nno one recollected ever having seen it. I took no copy when it was\r\nwritten. Long after the application of General Badeau, General\r\nTownsend, who had become Adjutant-General of the Army, while\r\npacking up papers preparatory to the removal of his office, found\r\nthis letter in some out-of-the-way place. It had not been\r\ndestroyed, but it had not been regularly filed away.', 'Paragraph 43': 'I felt some hesitation in suggesting rank as high as the\r\ncolonelcy of a regiment, feeling somewhat doubtful whether I would\r\nbe equal to the position. But I had seen nearly every colonel who\r\nhad been mustered in from the State of Illinois, and some from\r\nIndiana, and felt that if they could command a regiment properly,\r\nand with credit, I could also.', 'Paragraph 44': 'Having but little to do after the muster of the last of the\r\nregiments authorized by the State legislature, I asked and obtained\r\nof the governor leave of absence for a week to visit my parents in\r\nCovington, Kentucky, immediately opposite Cincinnati. General\r\nMcClellan had been made a major-general and had his headquarters at\r\nCincinnati. In reality I wanted to see him. I had known him\r\nslightly at West Point, where we served one year together, and in\r\nthe Mexican war. I was in hopes that when he saw me he would offer\r\nme a position on his staff. I called on two successive days at his\r\noffice but failed to see him on either occasion, and returned to\r\nSpringfield.', 'Paragraph 45': "While I was absent from the State capital on this occasion the\r\nPresident's second call for troops was issued. This time it was for\r\n300,000 men, for three years or the war. This brought into the\r\nUnited States service all the regiments then in the State service.\r\nThese had elected their officers from highest to lowest and were\r\naccepted with their organizations as they were, except in two\r\ninstances. A Chicago regiment, the 19th infantry, had elected a\r\nvery young man to the colonelcy. When it came to taking the field\r\nthe regiment asked to have another appointed colonel and the one\r\nthey had previously chosen made lieutenant-colonel. The 21st\r\nregiment of infantry, mustered in by me at Mattoon, refused to go\r\ninto the service with the colonel of their selection in any\r\nposition. While I was still absent Governor Yates appointed me\r\ncolonel of this latter regiment. A few days after I was in charge\r\nof it and in camp on the fair grounds near Springfield.", 'Paragraph 46': 'My regiment was composed in large part of young men of as good\r\nsocial position as any in their section of the State. It embraced\r\nthe sons of farmers, lawyers, physicians, politicians, merchants,\r\nbankers and ministers, and some men of maturer years who had filled\r\nsuch positions themselves. There were also men in it who could be\r\nled astray; and the colonel, elected by the votes of the regiment,\r\nhad proved to be fully capable of developing all there was in his\r\nmen of recklessness. It was said that he even went so far at times\r\nas to take the guard from their posts and go with them to the\r\nvillage near by and make a night of it. When there came a prospect\r\nof battle the regiment wanted to have some one else to lead them. I\r\nfound it very hard work for a few days to bring all the men into\r\nanything like subordination; but the great majority favored\r\ndiscipline, and by the application of a little regular army\r\npunishment all were reduced to as good discipline as one could\r\nask.', 'Paragraph 47': "The ten regiments which had volunteered in the State service for\r\nthirty days, it will be remembered, had done so with a pledge to go\r\ninto the National service if called upon within that time. When\r\nthey volunteered the government had only called for ninety days'\r\nenlistments. Men were called now for three years or the war. They\r\nfelt that this change of period released them from the obligation\r\nof re-volunteering. When I was appointed colonel, the 21st regiment\r\nwas still in the State service. About the time they were to be\r\nmustered into the United States service, such of them as would go,\r\ntwo members of Congress from the State, McClernand and Logan,\r\nappeared at the capital and I was introduced to them. I had never\r\nseen either of them before, but I had read a great deal about them,\r\nand particularly about Logan, in the newspapers. Both were\r\ndemocratic members of Congress, and Logan had been elected from the\r\nsouthern district of the State, where he had a majority of eighteen\r\nthousand over his Republican competitor. His district had been\r\nsettled originally by people from the Southern States, and at the\r\nbreaking out of secession they sympathized with the South. At the\r\nfirst outbreak of war some of them joined the Southern army; many\r\nothers were preparing to do so; others rode over the country at\r\nnight denouncing the Union, and made it as necessary to guard\r\nrailroad bridges over which National troops had to pass in southern\r\nIllinois, as it was in Kentucky or any of the border slave states.\r\nLogan's popularity in this district was unbounded. He knew almost\r\nenough of the people in it by their Christian names, to form an\r\nordinary congressional district. As he went in politics, so his\r\ndistrict was sure to go. The Republican papers had been demanding\r\nthat he should announce where he stood on the questions which at\r\nthat time engrossed the whole of public thought. Some were very\r\nbitter in their denunciations of his silence. Logan was not a man\r\nto be coerced into an utterance by threats. He did, however, come\r\nout in a speech before the adjournment of the special session of\r\nCongress which was convened by the President soon after his\r\ninauguration, and announced his undying loyalty and devotion to the\r\nUnion. But I had not happened to see that speech, so that when I\r\nfirst met Logan my impressions were those formed from reading\r\ndenunciations of him. McClernand, on the other hand, had early\r\ntaken strong grounds for the maintenance of the Union and had been\r\npraised accordingly by the Republican papers. The gentlemen who\r\npresented these two members of Congress asked me if I would have\r\nany objections to their addressing my regiment. I hesitated a\r\nlittle before answering. It was but a few days before the time set\r\nfor mustering into the United States service such of the men as\r\nwere willing to volunteer for three years or the war. I had some\r\ndoubt as to the effect a speech from Logan might have; but as he\r\nwas with McClernand, whose sentiments on the all-absorbing\r\nquestions of the day were well known, I gave my consent. McClernand\r\nspoke first; and Logan followed in a speech which he has hardly\r\nequalled since for force and eloquence. It breathed a loyalty and\r\ndevotion to the Union which inspired my men to such a point that\r\nthey would have volunteered to remain in the army as long as an\r\nenemy of the country continued to bear arms against it. They\r\nentered the United States service almost to a man.", 'Paragraph 48': 'General Logan went to his part of the State and gave his\r\nattention to raising troops. The very men who at first made it\r\nnecessary to guard the roads in southern Illinois became the\r\ndefenders of the Union. Logan entered the service himself as\r\ncolonel of a regiment and rapidly rose to the rank of\r\nmajor-general. His district, which had promised at first to give\r\nmuch trouble to the government, filled every call made upon it for\r\ntroops, without resorting to the draft. There was no call made when\r\nthere were not more volunteers than were asked for. That\r\ncongressional district stands credited at the War Department to-day\r\nwith furnishing more men for the army than it was called on to\r\nsupply.', 'Paragraph 49': 'I remained in Springfield with my regiment until the 3d of July,\r\nwhen I was ordered to Quincy, Illinois. By that time the regiment\r\nwas in a good state of discipline and the officers and men were\r\nwell up in the company drill. There was direct railroad\r\ncommunication between Springfield and Quincy, but I thought it\r\nwould be good preparation for the troops to march there. We had no\r\ntransportation for our camp and garrison equipage, so wagons were\r\nhired for the occasion and on the 3d of July we started. There was\r\nno hurry, but fair marches were made every day until the Illinois\r\nRiver was crossed. There I was overtaken by a dispatch saying that\r\nthe destination of the regiment had been changed to Ironton,\r\nMissouri, and ordering me to halt where I was and await the arrival\r\nof a steamer which had been dispatched up the Illinois River to\r\ntake the regiment to St. Louis. The boat, when it did come,\r\ngrounded on a sand-bar a few miles below where we were in camp. We\r\nremained there several days waiting to have the boat get off the\r\nbar, but before this occurred news came that an Illinois regiment\r\nwas surrounded by rebels at a point on the Hannibal and St. Joe\r\nRailroad some miles west of Palmyra, in Missouri, and I was ordered\r\nto proceed with all dispatch to their relief. We took the cars and\r\nreached Quincy in a few hours.', 'Paragraph 50': 'When I left Galena for the last time to take command of the 21st\r\nregiment I took with me my oldest son, Frederick D. Grant, then a\r\nlad of eleven years of age. On receiving the order to take rail for\r\nQuincy I wrote to Mrs. Grant, to relieve what I supposed would be\r\nher great anxiety for one so young going into danger, that I would\r\nsend Fred home from Quincy by river. I received a prompt letter in\r\nreply decidedly disapproving my proposition, and urging that the\r\nlad should be allowed to accompany me. It came too late. Fred was\r\nalready on his way up the Mississippi bound for Dubuque, Iowa, from\r\nwhich place there was a railroad to Galena.', 'Paragraph 51': 'My sensations as we approached what I supposed might be "a field\r\nof battle" were anything but agreeable. I had been in all the\r\nengagements in Mexico that it was possible for one person to be in;\r\nbut not in command. If some one else had been colonel and I had\r\nbeen lieutenant-colonel I do not think I would have felt any\r\ntrepidation. Before we were prepared to cross the Mississippi River\r\nat Quincy my anxiety was relieved; for the men of the besieged\r\nregiment came straggling into town. I am inclined to think both\r\nsides got frightened and ran away.', 'Paragraph 52': 'I took my regiment to Palmyra and remained there for a few days,\r\nuntil relieved by the 19th Illinois infantry. From Palmyra I\r\nproceeded to Salt River, the railroad bridge over which had been\r\ndestroyed by the enemy. Colonel John M. Palmer at that time\r\ncommanded the 13th Illinois, which was acting as a guard to workmen\r\nwho were engaged in rebuilding this bridge. Palmer was my senior\r\nand commanded the two regiments as long as we remained together.\r\nThe bridge was finished in about two weeks, and I received orders\r\nto move against Colonel Thomas Harris, who was said to be encamped\r\nat the little town of Florida, some twenty-five miles south of\r\nwhere we then were.', 'Paragraph 53': "At the time of which I now write we had no transportation and\r\nthe country about Salt River was sparsely settled, so that it took\r\nsome days to collect teams and drivers enough to move the camp and\r\ngarrison equipage of a regiment nearly a thousand strong, together\r\nwith a week's supply of provision and some ammunition. While\r\npreparations for the move were going on I felt quite comfortable;\r\nbut when we got on the road and found every house deserted I was\r\nanything but easy. In the twenty-five miles we had to march we did\r\nnot see a person, old or young, male or female, except two horsemen\r\nwho were on a road that crossed ours. As soon as they saw us they\r\ndecamped as fast as their horses could carry them. I kept my men in\r\nthe ranks and forbade their entering any of the deserted houses or\r\ntaking anything from them. We halted at night on the road and\r\nproceeded the next morning at an early hour. Harris had been\r\nencamped in a creek bottom for the sake of being near water. The\r\nhills on either side of the creek extend to a considerable height,\r\npossibly more than a hundred feet. As we approached the brow of the\r\nhill from which it was expected we could see Harris' camp, and\r\npossibly find his men ready formed to meet us, my heart kept\r\ngetting higher and higher until it felt to me as though it was in\r\nmy throat. I would have given anything then to have been back in\r\nIllinois, but I had not the moral courage to halt and consider what\r\nto do; I kept right on. When we reached a point from which the\r\nvalley below was in full view I halted. The place where Harris had\r\nbeen encamped a few days before was still there and the marks of a\r\nrecent encampment were plainly visible, but the troops were gone.\r\nMy heart resumed its place. It occurred to me at once that Harris\r\nhad been as much afraid of me as I had been of him. This was a view\r\nof the question I had never taken before; but it was one I never\r\nforgot afterwards. From that event to the close of the war, I never\r\nexperienced trepidation upon confronting an enemy, though I always\r\nfelt more or less anxiety. I never forgot that he had as much\r\nreason to fear my forces as I had his. The lesson was valuable.", 'Paragraph 54': 'Inquiries at the village of Florida divulged the fact that\r\nColonel Harris, learning of my intended movement, while my\r\ntransportation was being collected took time by the forelock and\r\nleft Florida before I had started from Salt River. He had increased\r\nthe distance between us by forty miles. The next day I started back\r\nto my old camp at Salt River bridge. The citizens living on the\r\nline of our march had returned to their houses after we passed, and\r\nfinding everything in good order, nothing carried away, they were\r\nat their front doors ready to greet us now. They had evidently been\r\nled to believe that the National troops carried death and\r\ndevastation with them wherever they went.', 'Paragraph 55': 'In a short time after our return to Salt River bridge I was\r\nordered with my regiment to the town of Mexico. General Pope was\r\nthen commanding the district embracing all of the State of Missouri\r\nbetween the Mississippi and Missouri rivers, with his headquarters\r\nin the village of Mexico. I was assigned to the command of a\r\nsub-district embracing the troops in the immediate neighborhood,\r\nsome three regiments of infantry and a section of artillery. There\r\nwas one regiment encamped by the side of mine. I assumed command of\r\nthe whole and the first night sent the commander of the other\r\nregiment the parole and countersign. Not wishing to be outdone in\r\ncourtesy, he immediately sent me the countersign for his regiment\r\nfor the night. When he was informed that the countersign sent to\r\nhim was for use with his regiment as well as mine, it was difficult\r\nto make him understand that this was not an unwarranted\r\ninterference of one colonel over another. No doubt he attributed it\r\nfor the time to the presumption of a graduate of West Point over a\r\nvolunteer pure and simple. But the question was soon settled and we\r\nhad no further trouble.', 'Paragraph 56': 'My arrival in Mexico had been preceded by that of two or three\r\nregiments in which proper discipline had not been maintained, and\r\nthe men had been in the habit of visiting houses without invitation\r\nand helping themselves to food and drink, or demanding them from\r\nthe occupants. They carried their muskets while out of camp and\r\nmade every man they found take the oath of allegiance to the\r\ngovernment. I at once published orders prohibiting the soldiers\r\nfrom going into private houses unless invited by the inhabitants,\r\nand from appropriating private property to their own or to\r\ngovernment uses. The people were no longer molested or made afraid.\r\nI received the most marked courtesy from the citizens of Mexico as\r\nlong as I remained there.', 'Paragraph 57': "Up to this time my regiment had not been carried in the school\r\nof the soldier beyond the company drill, except that it had\r\nreceived some training on the march from Springfield to the\r\nIllinois River. There was now a good opportunity of exercising it\r\nin the battalion drill. While I was at West Point the tactics used\r\nin the army had been Scott's and the musket the flint lock. I had\r\nnever looked at a copy of tactics from the time of my graduation.\r\nMy standing in that branch of studies had been near the foot of the\r\nclass. In the Mexican war in the summer of 1846, I had been\r\nappointed regimental quartermaster and commissary and had not been\r\nat a battalion drill since. The arms had been changed since then\r\nand Hardee's tactics had been adopted. I got a copy of tactics and\r\nstudied one lesson, intending to confine the exercise of the first\r\nday to the commands I had thus learned. By pursuing this course\r\nfrom day to day I thought I would soon get through the volume.", 'Paragraph 58': 'We were encamped just outside of town on the common, among\r\nscattering suburban houses with enclosed gardens, and when I got my\r\nregiment in line and rode to the front I soon saw that if I\r\nattempted to follow the lesson I had studied I would have to clear\r\naway some of the houses and garden fences to make room. I perceived\r\nat once, however, that Hardee\'s tactics—a mere translation\r\nfrom the French with Hardee\'s name attached—was nothing more\r\nthan common sense and the progress of the age applied to Scott\'s\r\nsystem. The commands were abbreviated and the movement expedited.\r\nUnder the old tactics almost every change in the order of march was\r\npreceded by a "halt," then came the change, and then the "forward\r\nmarch." With the new tactics all these changes could be made while\r\nin motion. I found no trouble in giving commands that would take my\r\nregiment where I wanted it to go and carry it around all obstacles.\r\nI do not believe that the officers of the regiment ever discovered\r\nthat I had never studied the tactics that I used.', 'Paragraph 59': 'I had not been in Mexico many weeks when, reading a St. Louis\r\npaper, I found the President had asked the Illinois delegation in\r\nCongress to recommend some citizens of the State for the position\r\nof brigadier-general, and that they had unanimously recommended me\r\nas first on a list of seven. I was very much surprised because, as\r\nI have said, my acquaintance with the Congressmen was very limited\r\nand I did not know of anything I had done to inspire such\r\nconfidence. The papers of the next day announced that my name, with\r\nthree others, had been sent to the Senate, and a few days after our\r\nconfirmation was announced.', 'Paragraph 60': 'When appointed brigadier-general I at once thought it proper\r\nthat one of my aides should come from the regiment I had been\r\ncommanding, and so selected Lieutenant C. B. Lagow. While living in\r\nSt. Louis, I had had a desk in the law office of McClellan, Moody\r\nand Hillyer. Difference in views between the members of the firm on\r\nthe questions of the day, and general hard times in the border\r\ncities, had broken up this firm. Hillyer was quite a young man,\r\nthen in his twenties, and very brilliant. I asked him to accept a\r\nplace on my staff. I also wanted to take one man from my new home,\r\nGalena. The canvass in the Presidential campaign the fall before\r\nhad brought out a young lawyer by the name of John A. Rawlins, who\r\nproved himself one of the ablest speakers in the State. He was also\r\na candidate for elector on the Douglas ticket. When Sumter was\r\nfired upon and the integrity of the Union threatened, there was no\r\nman more ready to serve his country than he. I wrote at once asking\r\nhim to accept the position of assistant adjutant-general with the\r\nrank of captain, on my staff. He was about entering the service as\r\nmajor of a new regiment then organizing in the north-western part\r\nof the State; but he threw this up and accepted my offer.', 'Paragraph 61': 'Neither Hillyer nor Lagow proved to have any particular taste or\r\nspecial qualifications for the duties of the soldier, and the\r\nformer resigned during the Vicksburg campaign; the latter I\r\nrelieved after the battle of Chattanooga. Rawlins remained with me\r\nas long as he lived, and rose to the rank of brigadier general and\r\nchief-of-staff to the General of the Army—an office created\r\nfor him—before the war closed. He was an able man, possessed\r\nof great firmness, and could say "no" so emphatically to a request\r\nwhich he thought should not be granted that the person he was\r\naddressing would understand at once that there was no use of\r\npressing the matter. General Rawlins was a very useful officer in\r\nother ways than this. I became very much attached to him.', 'Paragraph 62': "Shortly after my promotion I was ordered to Ironton, Missouri,\r\nto command a district in that part of the State, and took the 21st\r\nIllinois, my old regiment, with me. Several other regiments were\r\nordered to the same destination about the same time. Ironton is on\r\nthe Iron Mountain railroad, about seventy miles south of St. Louis,\r\nand situated among hills rising almost to the dignity of mountains.\r\nWhen I reached there, about the 8th of August, Colonel B. Gratz\r\nBrown—afterwards Governor of Missouri and in 1872\r\nVice-Presidential candidate—was in command. Some of his\r\ntroops were ninety days' men and their time had expired some time\r\nbefore. The men had no clothing but what they had volunteered in,\r\nand much of this was so worn that it would hardly stay on. General\r\nHardee—the author of the tactics I did not study—was at\r\nGreenville some twenty-five miles further south, it was said, with\r\nfive thousand Confederate troops. Under these circumstances Colonel\r\nBrown's command was very much demoralized. A squadron of cavalry\r\ncould have ridden into the valley and captured the entire force.\r\nBrown himself was gladder to see me on that occasion than he ever\r\nhas been since. I relieved him and sent all his men home within a\r\nday or two, to be mustered out of service.", 'Paragraph 63': 'Within ten days after reaching Ironton I was prepared to take the\r\noffensive against the enemy at Greenville. I sent a column east out\r\nof the valley we were in, with orders to swing around to the south\r\nand west and come into the Greenville road ten miles south of\r\nIronton. Another column marched on the direct road and went into\r\ncamp at the point designated for the two columns to meet. I was to\r\nride out the next morning and take personal command of the\r\nmovement. My experience against Harris, in northern Missouri, had\r\ninspired me with confidence. But when the evening train came in, it\r\nbrought General B. M. Prentiss with orders to take command of the\r\ndistrict. His orders did not relieve me, but I knew that by law I\r\nwas senior, and at that time even the President did not have the\r\nauthority to assign a junior to command a senior of the same grade.\r\nI therefore gave General Prentiss the situation of the troops and\r\nthe general condition of affairs, and started for St. Louis the\r\nsame day. The movement against the rebels at Greenville went no\r\nfurther.', 'Paragraph 64': 'From St. Louis I was ordered to Jefferson City, the capital of\r\nthe State, to take command. General Sterling Price, of the\r\nConfederate army, was thought to be threatening the capital,\r\nLexington, Chillicothe and other comparatively large towns in the\r\ncentral part of Missouri. I found a good many troops in Jefferson\r\nCity, but in the greatest confusion, and no one person knew where\r\nthey all were. Colonel Mulligan, a gallant man, was in command, but\r\nhe had not been educated as yet to his new profession and did not\r\nknow how to maintain discipline. I found that volunteers had\r\nobtained permission from the department commander, or claimed they\r\nhad, to raise, some of them, regiments; some battalions; some\r\ncompanies—the officers to be commissioned according to the\r\nnumber of men they brought into the service. There were recruiting\r\nstations all over town, with notices, rudely lettered on boards\r\nover the doors, announcing the arm of service and length of time\r\nfor which recruits at that station would be received. The law\r\nrequired all volunteers to serve for three years or the war. But in\r\nJefferson City in August, 1861, they were recruited for different\r\nperiods and on different conditions; some were enlisted for six\r\nmonths, some for a year, some without any condition as to where\r\nthey were to serve, others were not to be sent out of the State.\r\nThe recruits were principally men from regiments stationed there\r\nand already in the service, bound for three years if the war lasted\r\nthat long.', 'Paragraph 65': 'The city was filled with Union fugitives who had been driven by\r\nguerilla bands to take refuge with the National troops. They were\r\nin a deplorable condition and must have starved but for the support\r\nthe government gave them. They had generally made their escape with\r\na team or two, sometimes a yoke of oxen with a mule or a horse in\r\nthe lead. A little bedding besides their clothing and some food had\r\nbeen thrown into the wagon. All else of their worldly goods were\r\nabandoned and appropriated by their former neighbors; for the Union\r\nman in Missouri who staid at home during the rebellion, if he was\r\nnot immediately under the protection of the National troops, was at\r\nperpetual war with his neighbors. I stopped the recruiting service,\r\nand disposed the troops about the outskirts of the city so as to\r\nguard all approaches. Order was soon restored.', 'Paragraph 66': 'I had been at Jefferson City but a few days when I was directed\r\nfrom department headquarters to fit out an expedition to Lexington,\r\nBooneville and Chillicothe, in order to take from the banks in\r\nthose cities all the funds they had and send them to St. Louis. The\r\nwestern army had not yet been supplied with transportation. It\r\nbecame necessary therefore to press into the service teams\r\nbelonging to sympathizers with the rebellion or to hire those of\r\nUnion men. This afforded an opportunity of giving employment to\r\nsuch of the refugees within our lines as had teams suitable for our\r\npurposes. They accepted the service with alacrity. As fast as\r\ntroops could be got off they were moved west some twenty miles or\r\nmore. In seven or eight days from my assuming command at Jefferson\r\nCity, I had all the troops, except a small garrison, at an advanced\r\nposition and expected to join them myself the next day.', 'Paragraph 67': 'But my campaigns had not yet begun, for while seated at my\r\noffice door, with nothing further to do until it was time to start\r\nfor the front, I saw an officer of rank approaching, who proved to\r\nbe Colonel Jefferson C. Davis. I had never met him before, but he\r\nintroduced himself by handing me an order for him to proceed to\r\nJefferson City and relieve me of the command. The orders directed\r\nthat I should report at department headquarters at St. Louis\r\nwithout delay, to receive important special instructions. It was\r\nabout an hour before the only regular train of the day would start.\r\nI therefore turned over to Colonel Davis my orders, and hurriedly\r\nstated to him the progress that had been made to carry out the\r\ndepartment instructions already described. I had at that time but\r\none staff officer, doing myself all the detail work usually\r\nperformed by an adjutant-general. In an hour after being relieved\r\nfrom the command I was on my way to St. Louis, leaving my single\r\nstaff officer [C. B. Lagow, the others not yet having joined me.]\r\nto follow the next day with our horses and baggage.', 'Paragraph 68': 'The "important special instructions" which I received the next\r\nday, assigned me to the command of the district of south-east\r\nMissouri, embracing all the territory south of St. Louis, in\r\nMissouri, as well as all southern Illinois. At first I was to take\r\npersonal command of a combined expedition that had been ordered for\r\nthe capture of Colonel Jeff. Thompson, a sort of independent or\r\npartisan commander who was disputing with us the possession of\r\nsouth-east Missouri. Troops had been ordered to move from Ironton\r\nto Cape Girardeau, sixty or seventy miles to the south-east, on the\r\nMississippi River; while the forces at Cape Girardeau had been\r\nordered to move to Jacksonville, ten miles out towards Ironton; and\r\ntroops at Cairo and Bird\'s Point, at the junction of the Ohio and\r\nMississippi rivers, were to hold themselves in readiness to go down\r\nthe Mississippi to Belmont, eighteen miles below, to be moved west\r\nfrom there when an officer should come to command them. I was the\r\nofficer who had been selected for this purpose. Cairo was to become\r\nmy headquarters when the expedition terminated.', 'Paragraph 69': 'In pursuance of my orders I established my temporary\r\nheadquarters at Cape Girardeau and sent instructions to the\r\ncommanding officer at Jackson, to inform me of the approach of\r\nGeneral Prentiss from Ironton. Hired wagons were kept moving night\r\nand day to take additional rations to Jackson, to supply the troops\r\nwhen they started from there. Neither General Prentiss nor Colonel\r\nMarsh, who commanded at Jackson, knew their destination. I drew up\r\nall the instructions for the contemplated move, and kept them in my\r\npocket until I should hear of the junction of our troops at\r\nJackson. Two or three days after my arrival at Cape Girardeau, word\r\ncame that General Prentiss was approaching that place (Jackson). I\r\nstarted at once to meet him there and to give him his orders. As I\r\nturned the first corner of a street after starting, I saw a column\r\nof cavalry passing the next street in front of me. I turned and\r\nrode around the block the other way, so as to meet the head of the\r\ncolumn. I found there General Prentiss himself, with a large\r\nescort. He had halted his troops at Jackson for the night, and had\r\ncome on himself to Cape Girardeau, leaving orders for his command\r\nto follow him in the morning. I gave the General his\r\norders—which stopped him at Jackson—but he was very\r\nmuch aggrieved at being placed under another brigadier-general,\r\nparticularly as he believed himself to be the senior. He had been a\r\nbrigadier, in command at Cairo, while I was mustering officer at\r\nSpringfield without any rank. But we were nominated at the same\r\ntime for the United States service, and both our commissions bore\r\ndate May 17th, 1861. By virtue of my former army rank I was, by\r\nlaw, the senior. General Prentiss failed to get orders to his\r\ntroops to remain at Jackson, and the next morning early they were\r\nreported as approaching Cape Girardeau. I then ordered the General\r\nvery peremptorily to countermarch his command and take it back to\r\nJackson. He obeyed the order, but bade his command adieu when he\r\ngot them to Jackson, and went to St. Louis and reported himself.\r\nThis broke up the expedition. But little harm was done, as Jeff.\r\nThompson moved light and had no fixed place for even nominal\r\nheadquarters. He was as much at home in Arkansas as he was in\r\nMissouri and would keep out of the way of a superior force.\r\nPrentiss was sent to another part of the State.', 'Paragraph 70': 'General Prentiss made a great mistake on the above occasion, one\r\nthat he would not have committed later in the war. When I came to\r\nknow him better, I regretted it much. In consequence of this\r\noccurrence he was off duty in the field when the principal campaign\r\nat the West was going on, and his juniors received promotion while\r\nhe was where none could be obtained. He would have been next to\r\nmyself in rank in the district of south-east Missouri, by virtue of\r\nhis services in the Mexican war. He was a brave and very earnest\r\nsoldier. No man in the service was more sincere in his devotion to\r\nthe cause for which we were battling; none more ready to make\r\nsacrifices or risk life in it.', 'Paragraph 71': "On the 4th of September I removed my headquarters to Cairo and\r\nfound Colonel Richard Oglesby in command of the post. We had never\r\nmet, at least not to my knowledge. After my promotion I had ordered\r\nmy brigadier-general's uniform from New York, but it had not yet\r\narrived, so that I was in citizen's dress. The Colonel had his\r\noffice full of people, mostly from the neighboring States of\r\nMissouri and Kentucky, making complaints or asking favors. He\r\nevidently did not catch my name when I was presented, for on my\r\ntaking a piece of paper from the table where he was seated and\r\nwriting the order assuming command of the district of south-east\r\nMissouri, Colonel Richard J. Oglesby to command the post at Bird's\r\nPoint, and handing it to him, he put on an expression of surprise\r\nthat looked a little as if he would like to have some one identify\r\nme. But he surrendered the office without question.", 'Paragraph 72': 'The day after I assumed command at Cairo a man came to me who\r\nsaid he was a scout of General Fremont. He reported that he had\r\njust come from Columbus, a point on the Mississippi twenty miles\r\nbelow on the Kentucky side, and that troops had started from there,\r\nor were about to start, to seize Paducah, at the mouth of the\r\nTennessee. There was no time for delay; I reported by telegraph to\r\nthe department commander the information I had received, and added\r\nthat I was taking steps to get off that night to be in advance of\r\nthe enemy in securing that important point. There was a large\r\nnumber of steamers lying at Cairo and a good many boatmen were\r\nstaying in the town. It was the work of only a few hours to get the\r\nboats manned, with coal aboard and steam up. Troops were also\r\ndesignated to go aboard. The distance from Cairo to Paducah is\r\nabout forty-five miles. I did not wish to get there before daylight\r\nof the 6th, and directed therefore that the boats should lie at\r\nanchor out in the stream until the time to start. Not having\r\nreceived an answer to my first dispatch, I again telegraphed to\r\ndepartment headquarters that I should start for Paducah that night\r\nunless I received further orders. Hearing nothing, we started\r\nbefore midnight and arrived early the following morning,\r\nanticipating the enemy by probably not over six or eight hours. It\r\nproved very fortunate that the expedition against Jeff. Thompson\r\nhad been broken up. Had it not been, the enemy would have seized\r\nPaducah and fortified it, to our very great annoyance.', 'Paragraph 73': 'When the National troops entered the town the citizens were\r\ntaken by surprise. I never after saw such consternation depicted on\r\nthe faces of the people. Men, women and children came out of their\r\ndoors looking pale and frightened at the presence of the invader.\r\nThey were expecting rebel troops that day. In fact, nearly four\r\nthousand men from Columbus were at that time within ten or fifteen\r\nmiles of Paducah on their way to occupy the place. I had but two\r\nregiments and one battery with me, but the enemy did not know this\r\nand returned to Columbus. I stationed my troops at the best points\r\nto guard the roads leading into the city, left gunboats to guard\r\nthe river fronts and by noon was ready to start on my return to\r\nCairo. Before leaving, however, I addressed a short printed\r\nproclamation to the citizens of Paducah assuring them of our\r\npeaceful intentions, that we had come among them to protect them\r\nagainst the enemies of our country, and that all who chose could\r\ncontinue their usual avocations with assurance of the protection of\r\nthe government. This was evidently a relief to them; but the\r\nmajority would have much preferred the presence of the other army.\r\nI reinforced Paducah rapidly from the troops at Cape Girardeau; and\r\na day or two later General C. F. Smith, a most accomplished\r\nsoldier, reported at Cairo and was assigned to the command of the\r\npost at the mouth of the Tennessee. In a short time it was well\r\nfortified and a detachment was sent to occupy Smithland, at the\r\nmouth of the Cumberland.', 'Paragraph 74': 'The State government of Kentucky at that time was rebel in\r\nsentiment, but wanted to preserve an armed neutrality between the\r\nNorth and the South, and the governor really seemed to think the\r\nState had a perfect right to maintain a neutral position. The\r\nrebels already occupied two towns in the State, Columbus and\r\nHickman, on the Mississippi; and at the very moment the National\r\ntroops were entering Paducah from the Ohio front, General Lloyd\r\nTilghman—a Confederate—with his staff and a small\r\ndetachment of men, were getting out in the other direction, while,\r\nas I have already said, nearly four thousand Confederate troops\r\nwere on Kentucky soil on their way to take possession of the town.\r\nBut, in the estimation of the governor and of those who thought\r\nwith him, this did not justify the National authorities in invading\r\nthe soil of Kentucky. I informed the legislature of the State of\r\nwhat I was doing, and my action was approved by the majority of\r\nthat body. On my return to Cairo I found authority from department\r\nheadquarters for me to take Paducah "if I felt strong enough," but\r\nvery soon after I was reprimanded from the same quarters for my\r\ncorrespondence with the legislature and warned against a repetition\r\nof the offence.', 'Paragraph 75': 'Soon after I took command at Cairo, General Fremont entered into\r\narrangements for the exchange of the prisoners captured at Camp\r\nJackson in the month of May. I received orders to pass them through\r\nmy lines to Columbus as they presented themselves with proper\r\ncredentials. Quite a number of these prisoners I had been\r\npersonally acquainted with before the war. Such of them as I had so\r\nknown were received at my headquarters as old acquaintances, and\r\nordinary routine business was not disturbed by their presence. On\r\none occasion when several were present in my office my intention to\r\nvisit Cape Girardeau the next day, to inspect the troops at that\r\npoint, was mentioned. Something transpired which postponed my trip;\r\nbut a steamer employed by the government was passing a point some\r\ntwenty or more miles above Cairo, the next day, when a section of\r\nrebel artillery with proper escort brought her to. A major, one of\r\nthose who had been at my headquarters the day before, came at once\r\naboard and after some search made a direct demand for my delivery.\r\nIt was hard to persuade him that I was not there. This officer was\r\nMajor Barrett, of St. Louis. I had been acquainted with his family\r\nbefore the war.', 'Paragraph 76': 'From the occupation of Paducah up to the early part of November\r\nnothing important occurred with the troops under my command. I was\r\nreinforced from time to time and the men were drilled and\r\ndisciplined preparatory for the service which was sure to come. By\r\nthe 1st of November I had not fewer than 20,000 men, most of them\r\nunder good drill and ready to meet any equal body of men who, like\r\nthemselves, had not yet been in an engagement. They were growing\r\nimpatient at lying idle so long, almost in hearing of the guns of\r\nthe enemy they had volunteered to fight against. I asked on one or\r\ntwo occasions to be allowed to move against Columbus. It could have\r\nbeen taken soon after the occupation of Paducah; but before\r\nNovember it was so strongly fortified that it would have required a\r\nlarge force and a long siege to capture it.', 'Paragraph 77': "In the latter part of October General Fremont took the field in\r\nperson and moved from Jefferson City against General Sterling\r\nPrice, who was then in the State of Missouri with a considerable\r\ncommand. About the first of November I was directed from department\r\nheadquarters to make a demonstration on both sides of the\r\nMississippi River with the view of detaining the rebels at Columbus\r\nwithin their lines. Before my troops could be got off, I was\r\nnotified from the same quarter that there were some 3,000 of the\r\nenemy on the St. Francis River about fifty miles west, or\r\nsouth-west, from Cairo, and was ordered to send another force\r\nagainst them. I dispatched Colonel Oglesby at once with troops\r\nsufficient to compete with the reported number of the enemy. On the\r\n5th word came from the same source that the rebels were about to\r\ndetach a large force from Columbus to be moved by boats down the\r\nMississippi and up the White River, in Arkansas, in order to\r\nreinforce Price, and I was directed to prevent this movement if\r\npossible. I accordingly sent a regiment from Bird's Point under\r\nColonel W. H. L. Wallace to overtake and reinforce Oglesby, with\r\norders to march to New Madrid, a point some distance below\r\nColumbus, on the Missouri side. At the same time I directed General\r\nC. F. Smith to move all the troops he could spare from Paducah\r\ndirectly against Columbus, halting them, however, a few miles from\r\nthe town to await further orders from me. Then I gathered up all\r\nthe troops at Cairo and Fort Holt, except suitable guards, and\r\nmoved them down the river on steamers convoyed by two gunboats,\r\naccompanying them myself. My force consisted of a little over 3,000\r\nmen and embraced five regiments of infantry, two guns and two\r\ncompanies of cavalry. We dropped down the river on the 6th to\r\nwithin about six miles of Columbus, debarked a few men on the\r\nKentucky side and established pickets to connect with the troops\r\nfrom Paducah.", 'Paragraph 78': "I had no orders which contemplated an attack by the National\r\ntroops, nor did I intend anything of the kind when I started out\r\nfrom Cairo; but after we started I saw that the officers and men\r\nwere elated at the prospect of at last having the opportunity of\r\ndoing what they had volunteered to do—fight the enemies of\r\ntheir country. I did not see how I could maintain discipline, or\r\nretain the confidence of my command, if we should return to Cairo\r\nwithout an effort to do something. Columbus, besides being strongly\r\nfortified, contained a garrison much more numerous than the force I\r\nhad with me. It would not do, therefore, to attack that point.\r\nAbout two o'clock on the morning of the 7th, I learned that the\r\nenemy was crossing troops from Columbus to the west bank to be\r\ndispatched, presumably, after Oglesby. I knew there was a small\r\ncamp of Confederates at Belmont, immediately opposite Columbus, and\r\nI speedily resolved to push down the river, land on the Missouri\r\nside, capture Belmont, break up the camp and return. Accordingly,\r\nthe pickets above Columbus were drawn in at once, and about\r\ndaylight the boats moved out from shore. In an hour we were\r\ndebarking on the west bank of the Mississippi, just out of range of\r\nthe batteries at Columbus.", 'Paragraph 79': 'The ground on the west shore of the river, opposite Columbus, is\r\nlow and in places marshy and cut up with sloughs. The soil is rich\r\nand the timber large and heavy. There were some small clearings\r\nbetween Belmont and the point where we landed, but most of the\r\ncountry was covered with the native forests. We landed in front of\r\na cornfield. When the debarkation commenced, I took a regiment down\r\nthe river to post it as a guard against surprise. At that time I\r\nhad no staff officer who could be trusted with that duty. In the\r\nwoods, at a short distance below the clearing, I found a\r\ndepression, dry at the time, but which at high water became a\r\nslough or bayou. I placed the men in the hollow, gave them their\r\ninstructions and ordered them to remain there until they were\r\nproperly relieved. These troops, with the gunboats, were to protect\r\nour transports.', 'Paragraph 80': 'Up to this time the enemy had evidently failed to divine our\r\nintentions. From Columbus they could, of course, see our gunboats\r\nand transports loaded with troops. But the force from Paducah was\r\nthreatening them from the land side, and it was hardly to be\r\nexpected that if Columbus was our object we would separate our\r\ntroops by a wide river. They doubtless thought we meant to draw a\r\nlarge force from the east bank, then embark ourselves, land on the\r\neast bank and make a sudden assault on Columbus before their\r\ndivided command could be united.', 'Paragraph 81': "About eight o'clock we started from the point of debarkation,\r\nmarching by the flank. After moving in this way for a mile or a\r\nmile and a half, I halted where there was marshy ground covered\r\nwith a heavy growth of timber in our front, and deployed a large\r\npart of my force as skirmishers. By this time the enemy discovered\r\nthat we were moving upon Belmont and sent out troops to meet us.\r\nSoon after we had started in line, his skirmishers were encountered\r\nand fighting commenced. This continued, growing fiercer and\r\nfiercer, for about four hours, the enemy being forced back\r\ngradually until he was driven into his camp. Early in this\r\nengagement my horse was shot under me, but I got another from one\r\nof my staff and kept well up with the advance until the river was\r\nreached.", 'Paragraph 82': 'The officers and men engaged at Belmont were then under fire for\r\nthe first time. Veterans could not have behaved better than they\r\ndid up to the moment of reaching the rebel camp. At this point they\r\nbecame demoralized from their victory and failed to reap its full\r\nreward. The enemy had been followed so closely that when he reached\r\nthe clear ground on which his camp was pitched he beat a hasty\r\nretreat over the river bank, which protected him from our shots and\r\nfrom view. This precipitate retreat at the last moment enabled the\r\nNational forces to pick their way without hinderance through the\r\nabatis—the only artificial defence the enemy had. The moment\r\nthe camp was reached our men laid down their arms and commenced\r\nrummaging the tents to pick up trophies. Some of the higher\r\nofficers were little better than the privates. They galloped about\r\nfrom one cluster of men to another and at every halt delivered a\r\nshort eulogy upon the Union cause and the achievements of the\r\ncommand.', 'Paragraph 83': 'All this time the troops we had been engaged with for four\r\nhours, lay crouched under cover of the river bank, ready to come up\r\nand surrender if summoned to do so; but finding that they were not\r\npursued, they worked their way up the river and came up on the bank\r\nbetween us and our transports. I saw at the same time two steamers\r\ncoming from the Columbus side towards the west shore, above us,\r\nblack—or gray—with soldiers from boiler-deck to roof.\r\nSome of my men were engaged in firing from captured guns at empty\r\nsteamers down the river, out of range, cheering at every shot. I\r\ntried to get them to turn their guns upon the loaded steamers above\r\nand not so far away. My efforts were in vain. At last I directed my\r\nstaff officers to set fire to the camps. This drew the fire of the\r\nenemy\'s guns located on the heights of Columbus. They had abstained\r\nfrom firing before, probably because they were afraid of hitting\r\ntheir own men; or they may have supposed, until the camp was on\r\nfire, that it was still in the possession of their friends. About\r\nthis time, too, the men we had driven over the bank were seen in\r\nline up the river between us and our transports. The alarm\r\n"surrounded" was given. The guns of the enemy and the report of\r\nbeing surrounded, brought officers and men completely under\r\ncontrol. At first some of the officers seemed to think that to be\r\nsurrounded was to be placed in a hopeless position, where there was\r\nnothing to do but surrender. But when I announced that we had cut\r\nour way in and could cut our way out just as well, it seemed a new\r\nrevelation to officers and soldiers. They formed line rapidly and\r\nwe started back to our boats, with the men deployed as skirmishers\r\nas they had been on entering camp. The enemy was soon encountered,\r\nbut his resistance this time was feeble. Again the Confederates\r\nsought shelter under the river banks. We could not stop, however,\r\nto pick them up, because the troops we had seen crossing the river\r\nhad debarked by this time and were nearer our transports than we\r\nwere. It would be prudent to get them behind us; but we were not\r\nagain molested on our way to the boats.', 'Paragraph 84': 'From the beginning of the fighting our wounded had been carried\r\nto the houses at the rear, near the place of debarkation. I now set\r\nthe troops to bringing their wounded to the boats. After this had\r\ngone on for some little time I rode down the road, without even a\r\nstaff officer, to visit the guard I had stationed over the approach\r\nto our transports. I knew the enemy had crossed over from Columbus\r\nin considerable numbers and might be expected to attack us as we\r\nwere embarking. This guard would be encountered first and, as they\r\nwere in a natural intrenchment, would be able to hold the enemy for\r\na considerable time. My surprise was great to find there was not a\r\nsingle man in the trench. Riding back to the boat I found the\r\nofficer who had commanded the guard and learned that he had\r\nwithdrawn his force when the main body fell back. At first I\r\nordered the guard to return, but finding that it would take some\r\ntime to get the men together and march them back to their position,\r\nI countermanded the order. Then fearing that the enemy we had seen\r\ncrossing the river below might be coming upon us unawares, I rode\r\nout in the field to our front, still entirely alone, to observe\r\nwhether the enemy was passing. The field was grown up with corn so\r\ntall and thick as to cut off the view of even a person on\r\nhorseback, except directly along the rows. Even in that direction,\r\nowing to the overhanging blades of corn, the view was not\r\nextensive. I had not gone more than a few hundred yards when I saw\r\na body of troops marching past me not fifty yards away. I looked at\r\nthem for a moment and then turned my horse towards the river and\r\nstarted back, first in a walk, and when I thought myself concealed\r\nfrom the view of the enemy, as fast as my horse could carry me.\r\nWhen at the river bank I still had to ride a few hundred yards to\r\nthe point where the nearest transport lay.', 'Paragraph 85': 'The cornfield in front of our transports terminated at the edge\r\nof a dense forest. Before I got back the enemy had entered this\r\nforest and had opened a brisk fire upon the boats. Our men, with\r\nthe exception of details that had gone to the front after the\r\nwounded, were now either aboard the transports or very near them.\r\nThose who were not aboard soon got there, and the boats pushed off.\r\nI was the only man of the National army between the rebels and our\r\ntransports. The captain of a boat that had just pushed out but had\r\nnot started, recognized me and ordered the engineer not to start\r\nthe engine; he then had a plank run out for me. My horse seemed to\r\ntake in the situation. There was no path down the bank and every\r\none acquainted with the Mississippi River knows that its banks, in\r\na natural state, do not vary at any great angle from the\r\nperpendicular. My horse put his fore feet over the bank without\r\nhesitation or urging, and with his hind feet well under him, slid\r\ndown the bank and trotted aboard the boat, twelve or fifteen feet\r\naway, over a single gang plank. I dismounted and went at once to\r\nthe upper deck.', 'Paragraph 86': "The Mississippi River was low on the 7th of November, 1861, so\r\nthat the banks were higher than the heads of men standing on the\r\nupper decks of the steamers. The rebels were some distance back\r\nfrom the river, so that their fire was high and did us but little\r\nharm. Our smoke-stack was riddled with bullets, but there were only\r\nthree men wounded on the boats, two of whom were soldiers. When I\r\nfirst went on deck I entered the captain's room adjoining the\r\npilot-house, and threw myself on a sofa. I did not keep that\r\nposition a moment, but rose to go out on the deck to observe what\r\nwas going on. I had scarcely left when a musket ball entered the\r\nroom, struck the head of the sofa, passed through it and lodged in\r\nthe foot.", 'Paragraph 87': 'When the enemy opened fire on the transports our gunboats\r\nreturned it with vigor. They were well out in the stream and some\r\ndistance down, so that they had to give but very little elevation\r\nto their guns to clear the banks of the river. Their position very\r\nnearly enfiladed the line of the enemy while he was marching\r\nthrough the cornfield. The execution was very great, as we could\r\nsee at the time and as I afterwards learned more positively. We\r\nwere very soon out of range and went peacefully on our way to\r\nCairo, every man feeling that Belmont was a great victory and that\r\nhe had contributed his share to it.', 'Paragraph 88': 'Our loss at Belmont was 485 in killed, wounded and missing.\r\nAbout 125 of our wounded fell into the hands of the enemy. We\r\nreturned with 175 prisoners and two guns, and spiked four other\r\npieces. The loss of the enemy, as officially reported, was 642 men,\r\nkilled, wounded and missing. We had engaged about 2,500 men,\r\nexclusive of the guard left with the transports. The enemy had\r\nabout 7,000; but this includes the troops brought over from\r\nColumbus who were not engaged in the first defence of Belmont.', 'Paragraph 89': 'The two objects for which the battle of Belmont was fought were\r\nfully accomplished. The enemy gave up all idea of detaching troops\r\nfrom Columbus. His losses were very heavy for that period of the\r\nwar. Columbus was beset by people looking for their wounded or dead\r\nkin, to take them home for medical treatment or burial. I learned\r\nlater, when I had moved further south, that Belmont had caused more\r\nmourning than almost any other battle up to that time. The National\r\ntroops acquired a confidence in themselves at Belmont that did not\r\ndesert them through the war.', 'Paragraph 90': "The day after the battle I met some officers from General Polk's\r\ncommand, arranged for permission to bury our dead at Belmont and\r\nalso commenced negotiations for the exchange of prisoners. When our\r\nmen went to bury their dead, before they were allowed to land they\r\nwere conducted below the point where the enemy had engaged our\r\ntransports. Some of the officers expressed a desire to see the\r\nfield; but the request was refused with the statement that we had\r\nno dead there.", 'Paragraph 91': 'While on the truce-boat I mentioned to an officer, whom I had\r\nknown both at West Point and in the Mexican war, that I was in the\r\ncornfield near their troops when they passed; that I had been on\r\nhorseback and had worn a soldier\'s overcoat at the time. This\r\nofficer was on General Polk\'s staff. He said both he and the\r\ngeneral had seen me and that Polk had said to his men, "There is a\r\nYankee; you may try your marksmanship on him if you wish," but\r\nnobody fired at me.', 'Paragraph 92': 'Belmont was severely criticised in the North as a wholly\r\nunnecessary battle, barren of results, or the possibility of them\r\nfrom the beginning. If it had not been fought, Colonel Oglesby\r\nwould probably have been captured or destroyed with his three\r\nthousand men. Then I should have been culpable indeed.', 'Paragraph 93': 'While at Cairo I had frequent opportunities of meeting the rebel\r\nofficers of the Columbus garrison. They seemed to be very fond of\r\ncoming up on steamers under flags of truce. On two or three\r\noccasions I went down in like manner. When one of their boats was\r\nseen coming up carrying a white flag, a gun would be fired from the\r\nlower battery at Fort Holt, throwing a shot across the bow as a\r\nsignal to come no farther. I would then take a steamer and, with my\r\nstaff and occasionally a few other officers, go down to receive the\r\nparty. There were several officers among them whom I had known\r\nbefore, both at West Point and in Mexico. Seeing these officers who\r\nhad been educated for the profession of arms, both at school and in\r\nactual war, which is a far more efficient training, impressed me\r\nwith the great advantage the South possessed over the North at the\r\nbeginning of the rebellion. They had from thirty to forty per cent.\r\nof the educated soldiers of the Nation. They had no standing army\r\nand, consequently, these trained soldiers had to find employment\r\nwith the troops from their own States. In this way what there was\r\nof military education and training was distributed throughout their\r\nwhole army. The whole loaf was leavened.', 'Paragraph 94': 'The North had a great number of educated and trained soldiers,\r\nbut the bulk of them were still in the army and were retained,\r\ngenerally with their old commands and rank, until the war had\r\nlasted many months. In the Army of the Potomac there was what was\r\nknown as the "regular brigade," in which, from the commanding\r\nofficer down to the youngest second lieutenant, every one was\r\neducated to his profession. So, too, with many of the batteries;\r\nall the officers, generally four in number to each, were men\r\neducated for their profession. Some of these went into battle at\r\nthe beginning under division commanders who were entirely without\r\nmilitary training. This state of affairs gave me an idea which I\r\nexpressed while at Cairo; that the government ought to disband the\r\nregular army, with the exception of the staff corps, and notify the\r\ndisbanded officers that they would receive no compensation while\r\nthe war lasted except as volunteers. The register should be kept\r\nup, but the names of all officers who were not in the volunteer\r\nservice at the close, should be stricken from it.', 'Paragraph 95': 'On the 9th of November, two days after the battle of Belmont,\r\nMajor-General H. W. Halleck superseded General Fremont in command\r\nof the Department of the Missouri. The limits of his command took\r\nin Arkansas and west Kentucky east to the Cumberland River. From\r\nthe battle of Belmont until early in February, 1862, the troops\r\nunder my command did little except prepare for the long struggle\r\nwhich proved to be before them.', 'Paragraph 96': 'The enemy at this time occupied a line running from the\r\nMississippi River at Columbus to Bowling Green and Mill Springs,\r\nKentucky. Each of these positions was strongly fortified, as were\r\nalso points on the Tennessee and Cumberland rivers near the\r\nTennessee state line. The works on the Tennessee were called Fort\r\nHeiman and Fort Henry, and that on the Cumberland was Fort\r\nDonelson. At these points the two rivers approached within eleven\r\nmiles of each other. The lines of rifle pits at each place extended\r\nback from the water at least two miles, so that the garrisons were\r\nin reality only seven miles apart. These positions were of immense\r\nimportance to the enemy; and of course correspondingly important\r\nfor us to possess ourselves of. With Fort Henry in our hands we had\r\na navigable stream open to us up to Muscle Shoals, in Alabama. The\r\nMemphis and Charleston Railroad strikes the Tennessee at Eastport,\r\nMississippi, and follows close to the banks of the river up to the\r\nshoals. This road, of vast importance to the enemy, would cease to\r\nbe of use to them for through traffic the moment Fort Henry became\r\nours. Fort Donelson was the gate to Nashville—a place of\r\ngreat military and political importance—and to a rich country\r\nextending far east in Kentucky. These two points in our possession\r\nthe enemy would necessarily be thrown back to the Memphis and\r\nCharleston road, or to the boundary of the cotton states, and, as\r\nbefore stated, that road would be lost to them for through\r\ncommunication.', 'Paragraph 97': "The designation of my command had been changed after Halleck's\r\narrival, from the District of South-east Missouri to the District\r\nof Cairo, and the small district commanded by General C. F. Smith,\r\nembracing the mouths of the Tennessee and Cumberland rivers, had\r\nbeen added to my jurisdiction. Early in January, 1862, I was\r\ndirected by General McClellan, through my department commander, to\r\nmake a reconnoissance in favor of Brigadier-General Don Carlos\r\nBuell, who commanded the Department of the Ohio, with headquarters\r\nat Louisville, and who was confronting General S. B. Buckner with a\r\nlarger Confederate force at Bowling Green. It was supposed that\r\nBuell was about to make some move against the enemy, and my\r\ndemonstration was intended to prevent the sending of troops from\r\nColumbus, Fort Henry or Donelson to Buckner. I at once ordered\r\nGeneral Smith to send a force up the west bank of the Tennessee to\r\nthreaten forts Heiman and Henry; McClernand at the same time with a\r\nforce of 6,000 men was sent out into west Kentucky, threatening\r\nColumbus with one column and the Tennessee River with another. I\r\nwent with McClernand's command. The weather was very bad; snow and\r\nrain fell; the roads, never good in that section, were intolerable.\r\nWe were out more than a week splashing through the mud, snow and\r\nrain, the men suffering very much. The object of the expedition was\r\naccomplished. The enemy did not send reinforcements to Bowling\r\nGreen, and General George H. Thomas fought and won the battle of\r\nMill Springs before we returned.", 'Paragraph 98': "As a result of this expedition General Smith reported that he\r\nthought it practicable to capture Fort Heiman. This fort stood on\r\nhigh ground, completely commanding Fort Henry on the opposite side\r\nof the river, and its possession by us, with the aid of our\r\ngunboats, would insure the capture of Fort Henry. This report of\r\nSmith's confirmed views I had previously held, that the true line\r\nof operations for us was up the Tennessee and Cumberland rivers.\r\nWith us there, the enemy would be compelled to fall back on the\r\neast and west entirely out of the State of Kentucky. On the 6th of\r\nJanuary, before receiving orders for this expedition, I had asked\r\npermission of the general commanding the department to go to see\r\nhim at St. Louis. My object was to lay this plan of campaign before\r\nhim. Now that my views had been confirmed by so able a general as\r\nSmith, I renewed my request to go to St. Louis on what I deemed\r\nimportant military business. The leave was granted, but not\r\ngraciously. I had known General Halleck but very slightly in the\r\nold army, not having met him either at West Point or during the\r\nMexican war. I was received with so little cordiality that I\r\nperhaps stated the object of my visit with less clearness than I\r\nmight have done, and I had not uttered many sentences before I was\r\ncut short as if my plan was preposterous. I returned to Cairo very\r\nmuch crestfallen.", 'Paragraph 99': 'Flag-officer Foote commanded the little fleet of gunboats then\r\nin the neighborhood of Cairo and, though in another branch of the\r\nservice, was subject to the command of General Halleck. He and I\r\nconsulted freely upon military matters and he agreed with me\r\nperfectly as to the feasibility of the campaign up the Tennessee.\r\nNotwithstanding the rebuff I had received from my immediate chief,\r\nI therefore, on the 28th of January, renewed the suggestion by\r\ntelegraph that "if permitted, I could take and hold Fort Henry on\r\nthe Tennessee." This time I was backed by Flag-officer Foote, who\r\nsent a similar dispatch. On the 29th I wrote fully in support of\r\nthe proposition. On the 1st of February I received full\r\ninstructions from department headquarters to move upon Fort Henry.\r\nOn the 2d the expedition started.', 'Paragraph 100': 'In February, 1862, there were quite a good many steamers laid up\r\nat Cairo for want of employment, the Mississippi River being closed\r\nagainst navigation below that point. There were also many men in\r\nthe town whose occupation had been following the river in various\r\ncapacities, from captain down to deck hand But there were not\r\nenough of either boats or men to move at one time the 17,000 men I\r\nproposed to take with me up the Tennessee. I loaded the boats with\r\nmore than half the force, however, and sent General McClernand in\r\ncommand. I followed with one of the later boats and found\r\nMcClernand had stopped, very properly, nine miles below Fort Henry.\r\nSeven gunboats under Flag-officer Foote had accompanied the\r\nadvance. The transports we had with us had to return to Paducah to\r\nbring up a division from there, with General C. F. Smith in\r\ncommand.', 'Paragraph 101': 'Before sending the boats back I wanted to get the troops as near\r\nto the enemy as I could without coming within range of their guns.\r\nThere was a stream emptying into the Tennessee on the east side,\r\napparently at about long range distance below the fort. On account\r\nof the narrow water-shed separating the Tennessee and Cumberland\r\nrivers at that point, the stream must be insignificant at ordinary\r\nstages, but when we were there, in February, it was a torrent. It\r\nwould facilitate the investment of Fort Henry materially if the\r\ntroops could be landed south of that stream. To test whether this\r\ncould be done I boarded the gunboat Essex and requested Captain Wm.\r\nPorter commanding it, to approach the fort to draw its fire. After\r\nwe had gone some distance past the mouth of the stream we drew the\r\nfire of the fort, which fell much short of us. In consequence I had\r\nmade up my mind to return and bring the troops to the upper side of\r\nthe creek, when the enemy opened upon us with a rifled gun that\r\nsent shot far beyond us and beyond the stream. One shot passed very\r\nnear where Captain Porter and I were standing, struck the deck near\r\nthe stern, penetrated and passed through the cabin and so out into\r\nthe river. We immediately turned back, and the troops were debarked\r\nbelow the mouth of the creek.', 'Paragraph 102': "When the landing was completed I returned with the transports to\r\nPaducah to hasten up the balance of the troops. I got back on the\r\n5th with the advance, the remainder following as rapidly as the\r\nsteamers could carry them. At ten o'clock at night, on the 5th, the\r\nwhole command was not yet up. Being anxious to commence operations\r\nas soon as possible before the enemy could reinforce heavily, I\r\nissued my orders for an advance at 11 A.M. on the 6th. I felt sure\r\nthat all the troops would be up by that time.", 'Paragraph 103': "Fort Henry occupies a bend in the river which gave the guns in\r\nthe water battery a direct fire down the stream. The camp outside\r\nthe fort was intrenched, with rifle pits and outworks two miles\r\nback on the road to Donelson and Dover. The garrison of the fort\r\nand camp was about 2,800, with strong reinforcements from Donelson\r\nhalted some miles out. There were seventeen heavy guns in the fort.\r\nThe river was very high, the banks being overflowed except where\r\nthe bluffs come to the water's edge. A portion of the ground on\r\nwhich Fort Henry stood was two feet deep in water. Below, the water\r\nextended into the woods several hundred yards back from the bank on\r\nthe east side. On the west bank Fort Heiman stood on high ground,\r\ncompletely commanding Fort Henry. The distance from Fort Henry to\r\nDonelson is but eleven miles. The two positions were so important\r\nto the enemy, AS HE SAW HIS INTEREST, that it was natural to\r\nsuppose that reinforcements would come from every quarter from\r\nwhich they could be got. Prompt action on our part was\r\nimperative.", 'Paragraph 104': 'The plan was for the troops and gunboats to start at the same\r\nmoment. The troops were to invest the garrison and the gunboats to\r\nattack the fort at close quarters. General Smith was to land a\r\nbrigade of his division on the west bank during the night of the\r\n5th and get it in rear of Heiman.', 'Paragraph 105': 'At the hour designated the troops and gunboats started. General\r\nSmith found Fort Heiman had been evacuated before his men arrived.\r\nThe gunboats soon engaged the water batteries at very close\r\nquarters, but the troops which were to invest Fort Henry were\r\ndelayed for want of roads, as well as by the dense forest and the\r\nhigh water in what would in dry weather have been unimportant beds\r\nof streams. This delay made no difference in the result. On our\r\nfirst appearance Tilghman had sent his entire command, with the\r\nexception of about one hundred men left to man the guns in the\r\nfort, to the outworks on the road to Dover and Donelson, so as to\r\nhave them out of range of the guns of our navy; and before any\r\nattack on the 6th he had ordered them to retreat on Donelson. He\r\nstated in his subsequent report that the defence was intended\r\nsolely to give his troops time to make their escape.', 'Paragraph 106': 'Tilghman was captured with his staff and ninety men, as well as\r\nthe armament of the fort, the ammunition and whatever stores were\r\nthere. Our cavalry pursued the retreating column towards Donelson\r\nand picked up two guns and a few stragglers; but the enemy had so\r\nmuch the start, that the pursuing force did not get in sight of any\r\nexcept the stragglers.', 'Paragraph 107': 'All the gunboats engaged were hit many times. The damage,\r\nhowever, beyond what could be repaired by a small expenditure of\r\nmoney, was slight, except to the Essex. A shell penetrated the\r\nboiler of that vessel and exploded it, killing and wounding\r\nforty-eight men, nineteen of whom were soldiers who had been\r\ndetailed to act with the navy. On several occasions during the war\r\nsuch details were made when the complement of men with the navy was\r\ninsufficient for the duty before them. After the fall of Fort Henry\r\nCaptain Phelps, commanding the iron-clad Carondelet, at my request\r\nascended the Tennessee River and thoroughly destroyed the bridge of\r\nthe Memphis and Ohio Railroad.', 'Paragraph 108': 'I informed the department commander of our success at Fort Henry\r\nand that on the 8th I would take Fort Donelson. But the rain\r\ncontinued to fall so heavily that the roads became impassable for\r\nartillery and wagon trains. Then, too, it would not have been\r\nprudent to proceed without the gunboats. At least it would have\r\nbeen leaving behind a valuable part of our available force.', 'Paragraph 109': "On the 7th, the day after the fall of Fort Henry, I took my\r\nstaff and the cavalry—a part of one regiment—and made a\r\nreconnoissance to within about a mile of the outer line of works at\r\nDonelson. I had known General Pillow in Mexico, and judged that\r\nwith any force, no matter how small, I could march up to within\r\ngunshot of any intrenchments he was given to hold. I said this to\r\nthe officers of my staff at the time. I knew that Floyd was in\r\ncommand, but he was no soldier, and I judged that he would yield to\r\nPillow's pretensions. I met, as I expected, no opposition in making\r\nthe reconnoissance and, besides learning the topography of the\r\ncountry on the way and around Fort Donelson, found that there were\r\ntwo roads available for marching; one leading to the village of\r\nDover, the other to Donelson.", 'Paragraph 110': "Fort Donelson is two miles north, or down the river, from Dover.\r\nThe fort, as it stood in 1861, embraced about one hundred acres of\r\nland. On the east it fronted the Cumberland; to the north it faced\r\nHickman's creek, a small stream which at that time was deep and\r\nwide because of the back-water from the river; on the south was\r\nanother small stream, or rather a ravine, opening into the\r\nCumberland. This also was filled with back-water from the river.\r\nThe fort stood on high ground, some of it as much as a hundred feet\r\nabove the Cumberland. Strong protection to the heavy guns in the\r\nwater batteries had been obtained by cutting away places for them\r\nin the bluff. To the west there was a line of rifle pits some two\r\nmiles back from the river at the farthest point. This line ran\r\ngenerally along the crest of high ground, but in one place crossed\r\na ravine which opens into the river between the village and the\r\nfort. The ground inside and outside of this intrenched line was\r\nvery broken and generally wooded. The trees outside of the\r\nrifle-pits had been cut down for a considerable way out, and had\r\nbeen felled so that their tops lay outwards from the intrenchments.\r\nThe limbs had been trimmed and pointed, and thus formed an abatis\r\nin front of the greater part of the line. Outside of this\r\nintrenched line, and extending about half the entire length of it,\r\nis a ravine running north and south and opening into Hickman creek\r\nat a point north of the fort. The entire side of this ravine next\r\nto the works was one long abatis.", 'Paragraph 111': "General Halleck commenced his efforts in all quarters to get\r\nreinforcements to forward to me immediately on my departure from\r\nCairo. General Hunter sent men freely from Kansas, and a large\r\ndivision under General Nelson, from Buell's army, was also\r\ndispatched. Orders went out from the War Department to consolidate\r\nfragments of companies that were being recruited in the Western\r\nStates so as to make full companies, and to consolidate companies\r\ninto regiments. General Halleck did not approve or disapprove of my\r\ngoing to Fort Donelson. He said nothing whatever to me on the\r\nsubject. He informed Buell on the 7th that I would march against\r\nFort Donelson the next day; but on the 10th he directed me to\r\nfortify Fort Henry strongly, particularly to the land side, saying\r\nthat he forwarded me intrenching tools for that purpose. I received\r\nthis dispatch in front of Fort Donelson.", 'Paragraph 112': 'I was very impatient to get to Fort Donelson because I knew the\r\nimportance of the place to the enemy and supposed he would\r\nreinforce it rapidly. I felt that 15,000 men on the 8th would be\r\nmore effective than 50,000 a month later. I asked Flag-officer\r\nFoote, therefore, to order his gunboats still about Cairo to\r\nproceed up the Cumberland River and not to wait for those gone to\r\nEastport and Florence; but the others got back in time and we\r\nstarted on the 12th. I had moved McClernand out a few miles the\r\nnight before so as to leave the road as free as possible.', 'Paragraph 113': 'Just as we were about to start the first reinforcement reached\r\nme on transports. It was a brigade composed of six full regiments\r\ncommanded by Colonel Thayer, of Nebraska. As the gunboats were\r\ngoing around to Donelson by the Tennessee, Ohio and Cumberland\r\nrivers, I directed Thayer to turn about and go under their\r\nconvoy.', 'Paragraph 114': 'I started from Fort Henry with 15,000 men, including eight\r\nbatteries and part of a regiment of cavalry, and, meeting with no\r\nobstruction to detain us, the advance arrived in front of the enemy\r\nby noon. That afternoon and the next day were spent in taking up\r\nground to make the investment as complete as possible. General\r\nSmith had been directed to leave a portion of his division behind\r\nto guard forts Henry and Heiman. He left General Lew. Wallace with\r\n2,500 men. With the remainder of his division he occupied our left,\r\nextending to Hickman creek. McClernand was on the right and covered\r\nthe roads running south and south-west from Dover. His right\r\nextended to the back-water up the ravine opening into the\r\nCumberland south of the village. The troops were not intrenched,\r\nbut the nature of the ground was such that they were just as well\r\nprotected from the fire of the enemy as if rifle-pits had been\r\nthrown up. Our line was generally along the crest of ridges. The\r\nartillery was protected by being sunk in the ground. The men who\r\nwere not serving the guns were perfectly covered from fire on\r\ntaking position a little back from the crest. The greatest\r\nsuffering was from want of shelter. It was midwinter and during the\r\nsiege we had rain and snow, thawing and freezing alternately. It\r\nwould not do to allow camp-fires except far down the hill out of\r\nsight of the enemy, and it would not do to allow many of the troops\r\nto remain there at the same time. In the march over from Fort Henry\r\nnumbers of the men had thrown away their blankets and overcoats.\r\nThere was therefore much discomfort and absolute suffering.', 'Paragraph 115': "During the 12th and 13th, and until the arrival of Wallace and\r\nThayer on the 14th, the National forces, composed of but 15,000\r\nmen, without intrenchments, confronted an intrenched army of\r\n21,000, without conflict further than what was brought on by\r\nourselves. Only one gunboat had arrived. There was a little\r\nskirmishing each day, brought on by the movement of our troops in\r\nsecuring commanding positions; but there was no actual fighting\r\nduring this time except once, on the 13th, in front of McClernand's\r\ncommand. That general had undertaken to capture a battery of the\r\nenemy which was annoying his men. Without orders or authority he\r\nsent three regiments to make the assault. The battery was in the\r\nmain line of the enemy, which was defended by his whole army\r\npresent. Of course the assault was a failure, and of course the\r\nloss on our side was great for the number of men engaged. In this\r\nassault Colonel William Morrison fell badly wounded. Up to this\r\ntime the surgeons with the army had no difficulty in finding room\r\nin the houses near our line for all the sick and wounded; but now\r\nhospitals were overcrowded. Owing, however, to the energy and skill\r\nof the surgeons the suffering was not so great as it might have\r\nbeen. The hospital arrangements at Fort Donelson were as complete\r\nas it was possible to make them, considering the inclemency of the\r\nweather and the lack of tents, in a sparsely settled country where\r\nthe houses were generally of but one or two rooms.", 'Paragraph 116': 'On the return of Captain Walke to Fort Henry on the 10th, I had\r\nrequested him to take the vessels that had accompanied him on his\r\nexpedition up the Tennessee, and get possession of the Cumberland\r\nas far up towards Donelson as possible. He started without delay,\r\ntaking, however, only his own gunboat, the Carondelet, towed by the\r\nsteamer Alps. Captain Walke arrived a few miles below Donelson on\r\nthe 12th, a little after noon. About the time the advance of troops\r\nreached a point within gunshot of the fort on the land side, he\r\nengaged the water batteries at long range. On the 13th I informed\r\nhim of my arrival the day before and of the establishment of most\r\nof our batteries, requesting him at the same time to attack again\r\nthat day so that I might take advantage of any diversion. The\r\nattack was made and many shots fell within the fort, creating some\r\nconsternation, as we now know. The investment on the land side was\r\nmade as complete as the number of troops engaged would admit\r\nof.', 'Paragraph 117': "During the night of the 13th Flag-officer Foote arrived with the\r\niron-clads St. Louis, Louisville and Pittsburg and the wooden\r\ngunboats Tyler and Conestoga, convoying Thayer's brigade. On the\r\nmorning of the 14th Thayer was landed. Wallace, whom I had ordered\r\nover from Fort Henry, also arrived about the same time. Up to this\r\ntime he had been commanding a brigade belonging to the division of\r\nGeneral C. F. Smith. These troops were now restored to the division\r\nthey belonged to, and General Lew. Wallace was assigned to the\r\ncommand of a division composed of the brigade of Colonel Thayer and\r\nother reinforcements that arrived the same day. This new division\r\nwas assigned to the centre, giving the two flanking divisions an\r\nopportunity to close up and form a stronger line.", 'Paragraph 118': 'The plan was for the troops to hold the enemy within his lines,\r\nwhile the gunboats should attack the water batteries at close\r\nquarters and silence his guns if possible. Some of the gunboats\r\nwere to run the batteries, get above the fort and above the village\r\nof Dover. I had ordered a reconnoissance made with the view of\r\ngetting troops to the river above Dover in case they should be\r\nneeded there. That position attained by the gunboats it would have\r\nbeen but a question of time—and a very short time,\r\ntoo—when the garrison would have been compelled to\r\nsurrender.', 'Paragraph 119': 'By three in the afternoon of the 14th Flag-officer Foote was\r\nready, and advanced upon the water batteries with his entire fleet.\r\nAfter coming in range of the batteries of the enemy the advance was\r\nslow, but a constant fire was delivered from every gun that could\r\nbe brought to bear upon the fort. I occupied a position on shore\r\nfrom which I could see the advancing navy. The leading boat got\r\nwithin a very short distance of the water battery, not further off\r\nI think than two hundred yards, and I soon saw one and then another\r\nof them dropping down the river, visibly disabled. Then the whole\r\nfleet followed and the engagement closed for the day. The gunboat\r\nwhich Flag-officer Foote was on, besides having been hit about\r\nsixty times, several of the shots passing through near the\r\nwaterline, had a shot enter the pilot-house which killed the pilot,\r\ncarried away the wheel and wounded the flag-officer himself. The\r\ntiller-ropes of another vessel were carried away and she, too,\r\ndropped helplessly back. Two others had their pilot-houses so\r\ninjured that they scarcely formed a protection to the men at the\r\nwheel.', 'Paragraph 120': 'The enemy had evidently been much demoralized by the assault,\r\nbut they were jubilant when they saw the disabled vessels dropping\r\ndown the river entirely out of the control of the men on board. Of\r\ncourse I only witnessed the falling back of our gunboats and felt\r\nsad enough at the time over the repulse. Subsequent reports, now\r\npublished, show that the enemy telegraphed a great victory to\r\nRichmond. The sun went down on the night of the 14th of February,\r\n1862, leaving the army confronting Fort Donelson anything but\r\ncomforted over the prospects. The weather had turned intensely\r\ncold; the men were without tents and could not keep up fires where\r\nmost of them had to stay, and, as previously stated, many had\r\nthrown away their overcoats and blankets. Two of the strongest of\r\nour gunboats had been disabled, presumably beyond the possibility\r\nof rendering any present assistance. I retired this night not\r\nknowing but that I would have to intrench my position, and bring up\r\ntents for the men or build huts under the cover of the hills.', 'Paragraph 121': 'On the morning of the 15th, before it was yet broad day, a\r\nmessenger from Flag-officer Foote handed me a note, expressing a\r\ndesire to see me on the flag-ship and saying that he had been\r\ninjured the day before so much that he could not come himself to\r\nme. I at once made my preparations for starting. I directed my\r\nadjutant-general to notify each of the division commanders of my\r\nabsence and instruct them to do nothing to bring on an engagement\r\nuntil they received further orders, but to hold their positions.\r\nFrom the heavy rains that had fallen for days and weeks preceding\r\nand from the constant use of the roads between the troops and the\r\nlanding four to seven miles below, these roads had become cut up so\r\nas to be hardly passable. The intense cold of the night of the\r\n14th-15th had frozen the ground solid. This made travel on\r\nhorseback even slower than through the mud; but I went as fast as\r\nthe roads would allow.', 'Paragraph 122': 'When I reached the fleet I found the flag-ship was anchored out\r\nin the stream. A small boat, however, awaited my arrival and I was\r\nsoon on board with the flag-officer. He explained to me in short\r\nthe condition in which he was left by the engagement of the evening\r\nbefore, and suggested that I should intrench while he returned to\r\nMound City with his disabled boats, expressing at the time the\r\nbelief that he could have the necessary repairs made and be back in\r\nten days. I saw the absolute necessity of his gunboats going into\r\nhospital and did not know but I should be forced to the alternative\r\nof going through a siege. But the enemy relieved me from this\r\nnecessity.', 'Paragraph 123': "When I left the National line to visit Flag-officer Foote I had\r\nno idea that there would be any engagement on land unless I brought\r\nit on myself. The conditions for battle were much more favorable to\r\nus than they had been for the first two days of the investment.\r\nFrom the 12th to the 14th we had but 15,000 men of all arms and no\r\ngunboats. Now we had been reinforced by a fleet of six naval\r\nvessels, a large division of troops under General L. Wallace and\r\n2,500 men brought over from Fort Henry belonging to the division of\r\nC. F. Smith. The enemy, however, had taken the initiative. Just as\r\nI landed I met Captain Hillyer of my staff, white with fear, not\r\nfor his personal safety, but for the safety of the National troops.\r\nHe said the enemy had come out of his lines in full force and\r\nattacked and scattered McClernand's division, which was in full\r\nretreat. The roads, as I have said, were unfit for making fast\r\ntime, but I got to my command as soon as possible. The attack had\r\nbeen made on the National right. I was some four or five miles\r\nnorth of our left. The line was about three miles long. In reaching\r\nthe point where the disaster had occurred I had to pass the\r\ndivisions of Smith and Wallace. I saw no sign of excitement on the\r\nportion of the line held by Smith; Wallace was nearer the scene of\r\nconflict and had taken part in it. He had, at an opportune time,\r\nsent Thayer's brigade to the support of McClernand and thereby\r\ncontributed to hold the enemy within his lines.", 'Paragraph 124': "I saw everything favorable for us along the line of our left and\r\ncentre. When I came to the right appearances were different. The\r\nenemy had come out in full force to cut his way out and make his\r\nescape. McClernand's division had to bear the brunt of the attack\r\nfrom this combined force. His men had stood up gallantly until the\r\nammunition in their cartridge-boxes gave out. There was abundance\r\nof ammunition near by lying on the ground in boxes, but at that\r\nstage of the war it was not all of our commanders of regiments,\r\nbrigades, or even divisions, who had been educated up to the point\r\nof seeing that their men were constantly supplied with ammunition\r\nduring an engagement. When the men found themselves without\r\nammunition they could not stand up against troops who seemed to\r\nhave plenty of it. The division broke and a portion fled, but most\r\nof the men, as they were not pursued, only fell back out of range\r\nof the fire of the enemy. It must have been about this time that\r\nThayer pushed his brigade in between the enemy and those of our\r\ntroops that were without ammunition. At all events the enemy fell\r\nback within his intrenchments and was there when I got on the\r\nfield.", 'Paragraph 125': 'I saw the men standing in knots talking in the most excited\r\nmanner. No officer seemed to be giving any directions. The soldiers\r\nhad their muskets, but no ammunition, while there were tons of it\r\nclose at hand. I heard some of the men say that the enemy had come\r\nout with knapsacks, and haversacks filled with rations. They seemed\r\nto think this indicated a determination on his part to stay out and\r\nfight just as long as the provisions held out. I turned to Colonel\r\nJ. D. Webster, of my staff, who was with me, and said: "Some of our\r\nmen are pretty badly demoralized, but the enemy must be more so,\r\nfor he has attempted to force his way out, but has fallen back: the\r\none who attacks first now will be victorious and the enemy will\r\nhave to be in a hurry if he gets ahead of me." I determined to make\r\nthe assault at once on our left. It was clear to my mind that the\r\nenemy had started to march out with his entire force, except a few\r\npickets, and if our attack could be made on the left before the\r\nenemy could redistribute his forces along the line, we would find\r\nbut little opposition except from the intervening abatis. I\r\ndirected Colonel Webster to ride with me and call out to the men as\r\nwe passed: "Fill your cartridge-boxes, quick, and get into line;\r\nthe enemy is trying to escape and he must not be permitted to do\r\nso." This acted like a charm. The men only wanted some one to give\r\nthem a command. We rode rapidly to Smith\'s quarters, when I\r\nexplained the situation to him and directed him to charge the\r\nenemy\'s works in his front with his whole division, saying at the\r\nsame time that he would find nothing but a very thin line to\r\ncontend with. The general was off in an incredibly short time,\r\ngoing in advance himself to keep his men from firing while they\r\nwere working their way through the abatis intervening between them\r\nand the enemy. The outer line of rifle-pits was passed, and the\r\nnight of the 15th General Smith, with much of his division,\r\nbivouacked within the lines of the enemy. There was now no doubt\r\nbut that the Confederates must surrender or be captured the next\r\nday.', 'Paragraph 126': 'There seems from subsequent accounts to have been much\r\nconsternation, particularly among the officers of high rank, in\r\nDover during the night of the 15th. General Floyd, the commanding\r\nofficer, who was a man of talent enough for any civil position, was\r\nno soldier and, possibly, did not possess the elements of one. He\r\nwas further unfitted for command, for the reason that his\r\nconscience must have troubled him and made him afraid. As Secretary\r\nof War he had taken a solemn oath to maintain the Constitution of\r\nthe United States and to uphold the same against all its enemies.\r\nHe had betrayed that trust. As Secretary of War he was reported\r\nthrough the northern press to have scattered the little army the\r\ncountry had so that the most of it could be picked up in detail\r\nwhen secession occurred. About a year before leaving the Cabinet he\r\nhad removed arms from northern to southern arsenals. He continued\r\nin the Cabinet of President Buchanan until about the 1st of\r\nJanuary, 1861, while he was working vigilantly for the\r\nestablishment of a confederacy made out of United States territory.\r\nWell may he have been afraid to fall into the hands of National\r\ntroops. He would no doubt have been tried for misappropriating\r\npublic property, if not for treason, had he been captured. General\r\nPillow, next in command, was conceited, and prided himself much on\r\nhis services in the Mexican war. He telegraphed to General\r\nJohnston, at Nashville, after our men were within the rebel\r\nrifle-pits, and almost on the eve of his making his escape, that\r\nthe Southern troops had had great success all day. Johnston\r\nforwarded the dispatch to Richmond. While the authorities at the\r\ncapital were reading it Floyd and Pillow were fugitives.', 'Paragraph 127': "A council of war was held by the enemy at which all agreed that\r\nit would be impossible to hold out longer. General Buckner, who was\r\nthird in rank in the garrison but much the most capable soldier,\r\nseems to have regarded it a duty to hold the fort until the general\r\ncommanding the department, A. S. Johnston, should get back to his\r\nheadquarters at Nashville. Buckner's report shows, however, that he\r\nconsidered Donelson lost and that any attempt to hold the place\r\nlonger would be at the sacrifice of the command. Being assured that\r\nJohnston was already in Nashville, Buckner too agreed that\r\nsurrender was the proper thing. Floyd turned over the command to\r\nPillow, who declined it. It then devolved upon Buckner, who\r\naccepted the responsibility of the position. Floyd and Pillow took\r\npossession of all the river transports at Dover and before morning\r\nboth were on their way to Nashville, with the brigade formerly\r\ncommanded by Floyd and some other troops, in all about 3,000. Some\r\nmarched up the east bank of the Cumberland; others went on the\r\nsteamers. During the night Forrest also, with his cavalry and some\r\nother troops about a thousand in all, made their way out, passing\r\nbetween our right and the river. They had to ford or swim over the\r\nback-water in the little creek just south of Dover.", 'Paragraph 128': 'Before daylight General Smith brought to me the following letter\r\nfrom General Buckner:', 'Paragraph 129': '\r\nHEADQUARTERS, FORT DONELSON,\r\nFebruary 16, 1862.', 'Paragraph 130': "SIR:—In consideration of all the circumstances governing\r\nthe present situation of affairs at this station, I propose to the\r\nCommanding Officer of the Federal forces the appointment of\r\nCommissioners to agree upon terms of capitulation of the forces and\r\nfort under my command, and in that view suggest an armistice until\r\n12 o'clock to-day.", 'Paragraph 131': "I am, sir, very respectfully,\r\nYour ob't se'v't,\r\nS. B. BUCKNER,\r\nBrig. Gen. C. S. A.", 'Paragraph 132': "To Brigadier-General U. S. Grant,\r\nCom'ding U. S. Forces,\r\nNear Fort Donelson.\r\n", 'Paragraph 133': 'To this I responded as follows:', 'Paragraph 134': 'HEADQUARTERS ARMY IN THE FIELD,\r\nCamp near Donelson,\r\nFebruary 16, 1862.', 'Paragraph 135': 'General S. B. BUCKNER,\r\nConfederate Army.', 'Paragraph 136': 'SIR:—Yours of this date, proposing armistice and\r\nappointment of Commissioners to settle terms of capitulation, is\r\njust received. No terms except an unconditional and immediate\r\nsurrender can be accepted. I propose to move immediately upon your\r\nworks.', 'Paragraph 137': "I am, sir, very respectfully,\r\nYour ob't se'v't,\r\nU. S. GRANT,\r\nBrig. Gen.", 'Paragraph 138': 'To this I received the following reply:', 'Paragraph 139': 'HEADQUARTERS, DOVER, TENNESSEE,\r\nFebruary 16, 1862.', 'Paragraph 140': "To Brig. Gen'I U. S. GRANT,\r\nU. S. Army.", 'Paragraph 141': 'SIR:—The distribution of the forces under my command,\r\nincident to an unexpected change of commanders, and the\r\noverwhelming force under your command, compel me, notwithstanding\r\nthe brilliant success of the Confederate arms yesterday, to accept\r\nthe ungenerous and unchivalrous terms which you propose.', 'Paragraph 142': "I am, sir,\r\nYour very ob't se'v't,\r\nS. B. BUCKNER,\r\nBrig. Gen. C. S. A.", 'Paragraph 143': 'General Buckner, as soon as he had dispatched the first of the\r\nabove letters, sent word to his different commanders on the line of\r\nrifle-pits, notifying them that he had made a proposition looking\r\nto the surrender of the garrison, and directing them to notify\r\nNational troops in their front so that all fighting might be\r\nprevented. White flags were stuck at intervals along the line of\r\nrifle-pits, but none over the fort. As soon as the last letter from\r\nBuckner was received I mounted my horse and rode to Dover. General\r\nWallace, I found, had preceded me an hour or more. I presume that,\r\nseeing white flags exposed in his front, he rode up to see what\r\nthey meant and, not being fired upon or halted, he kept on until he\r\nfound himself at the headquarters of General Buckner.', 'Paragraph 144': 'I had been at West Point three years with Buckner and afterwards\r\nserved with him in the army, so that we were quite well acquainted.\r\nIn the course of our conversation, which was very friendly, he said\r\nto me that if he had been in command I would not have got up to\r\nDonelson as easily as I did. I told him that if he had been in\r\ncommand I should not have tried in the way I did: I had invested\r\ntheir lines with a smaller force than they had to defend them, and\r\nat the same time had sent a brigade full 5,000 strong, around by\r\nwater; I had relied very much upon their commander to allow me to\r\ncome safely up to the outside of their works. I asked General\r\nBuckner about what force he had to surrender. He replied that he\r\ncould not tell with any degree of accuracy; that all the sick and\r\nweak had been sent to Nashville while we were about Fort Henry;\r\nthat Floyd and Pillow had left during the night, taking many men\r\nwith them; and that Forrest, and probably others, had also escaped\r\nduring the preceding night: the number of casualties he could not\r\ntell; but he said I would not find fewer than 12,000, nor more than\r\n15,000.', 'Paragraph 145': 'He asked permission to send parties outside of the lines to bury\r\nhis dead, who had fallen on the 15th when they tried to get out. I\r\ngave directions that his permit to pass our limits should be\r\nrecognized. I have no reason to believe that this privilege was\r\nabused, but it familiarized our guards so much with the sight of\r\nConfederates passing to and fro that I have no doubt many got\r\nbeyond our pickets unobserved and went on. The most of the men who\r\nwent in that way no doubt thought they had had war enough, and left\r\nwith the intention of remaining out of the army. Some came to me\r\nand asked permission to go, saying that they were tired of the war\r\nand would not be caught in the ranks again, and I bade them go.', 'Paragraph 146': "The actual number of Confederates at Fort Donelson can never be\r\ngiven with entire accuracy. The largest number admitted by any\r\nwriter on the Southern side, is by Colonel Preston Johnston. He\r\ngives the number at 17,000. But this must be an underestimate. The\r\ncommissary general of prisoners reported having issued rations to\r\n14,623 Fort Donelson prisoners at Cairo, as they passed that point.\r\nGeneral Pillow reported the killed and wounded at 2,000; but he had\r\nless opportunity of knowing the actual numbers than the officers of\r\nMcClernand's division, for most of the killed and wounded fell\r\noutside their works, in front of that division, and were buried or\r\ncared for by Buckner after the surrender and when Pillow was a\r\nfugitive. It is known that Floyd and Pillow escaped during the\r\nnight of the 15th, taking with them not less than 3,000 men.\r\nForrest escaped with about 1,000 and others were leaving singly and\r\nin squads all night. It is probable that the Confederate force at\r\nDonelson, on the 15th of February, 1862, was 21,000 in round\r\nnumbers.", 'Paragraph 147': 'On the day Fort Donelson fell I had 27,000 men to confront the\r\nConfederate lines and guard the road four or five miles to the\r\nleft, over which all our supplies had to be drawn on wagons. During\r\nthe 16th, after the surrender, additional reinforcements\r\narrived.', 'Paragraph 148': 'During the siege General Sherman had been sent to Smithland, at\r\nthe mouth of the Cumberland River, to forward reinforcements and\r\nsupplies to me. At that time he was my senior in rank and there was\r\nno authority of law to assign a junior to command a senior of the\r\nsame grade. But every boat that came up with supplies or\r\nreinforcements brought a note of encouragement from Sherman, asking\r\nme to call upon him for any assistance he could render and saying\r\nthat if he could be of service at the front I might send for him\r\nand he would waive rank.', 'Paragraph 149': 'The news of the fall of Fort Donelson caused great delight all\r\nover the North. At the South, particularly in Richmond, the effect\r\nwas correspondingly depressing. I was promptly promoted to the\r\ngrade of Major-General of Volunteers, and confirmed by the Senate.\r\nAll three of my division commanders were promoted to the same grade\r\nand the colonels who commanded brigades were made\r\nbrigadier-generals in the volunteer service. My chief, who was in\r\nSt. Louis, telegraphed his congratulations to General Hunter in\r\nKansas for the services he had rendered in securing the fall of\r\nFort Donelson by sending reinforcements so rapidly. To Washington\r\nhe telegraphed that the victory was due to General C. F. Smith;\r\n"promote him," he said, "and the whole country will applaud." On\r\nthe 19th there was published at St. Louis a formal order thanking\r\nFlag-officer Foote and myself, and the forces under our command,\r\nfor the victories on the Tennessee and the Cumberland. I received\r\nno other recognition whatever from General Halleck. But General\r\nCullum, his chief of staff, who was at Cairo, wrote me a warm\r\ncongratulatory letter on his own behalf. I approved of General\r\nSmith\'s promotion highly, as I did all the promotions that were\r\nmade.', 'Paragraph 150': 'My opinion was and still is that immediately after the fall of\r\nFort Donelson the way was opened to the National forces all over\r\nthe South-west without much resistance. If one general who would\r\nhave taken the responsibility had been in command of all the troops\r\nwest of the Alleghanies, he could have marched to Chattanooga,\r\nCorinth, Memphis and Vicksburg with the troops we then had, and as\r\nvolunteering was going on rapidly over the North there would soon\r\nhave been force enough at all these centres to operate offensively\r\nagainst any body of the enemy that might be found near them. Rapid\r\nmovements and the acquisition of rebellious territory would have\r\npromoted volunteering, so that reinforcements could have been had\r\nas fast as transportation could have been obtained to carry them to\r\ntheir destination. On the other hand there were tens of thousands\r\nof strong able-bodied young men still at their homes in the\r\nSouth-western States, who had not gone into the Confederate army in\r\nFebruary, 1862, and who had no particular desire to go. If our\r\nlines had been extended to protect their homes, many of them never\r\nwould have gone. Providence ruled differently. Time was given the\r\nenemy to collect armies and fortify his new positions; and twice\r\nafterwards he came near forcing his north-western front up to the\r\nOhio River.', 'Paragraph 151': 'I promptly informed the department commander of our success at\r\nFort Donelson and that the way was open now to Clarksville and\r\nNashville; and that unless I received orders to the contrary I\r\nshould take Clarksville on the 21st and Nashville about the 1st of\r\nMarch. Both these places are on the Cumberland River above Fort\r\nDonelson. As I heard nothing from headquarters on the subject,\r\nGeneral C. F. Smith was sent to Clarksville at the time designated\r\nand found the place evacuated. The capture of forts Henry and\r\nDonelson had broken the line the enemy had taken from Columbus to\r\nBowling Green, and it was known that he was falling back from the\r\neastern point of this line and that Buell was following, or at\r\nleast advancing. I should have sent troops to Nashville at the time\r\nI sent to Clarksville, but my transportation was limited and there\r\nwere many prisoners to be forwarded north.', 'Paragraph 152': "None of the reinforcements from Buell's army arrived until the\r\n24th of February. Then General Nelson came up, with orders to\r\nreport to me with two brigades, he having sent one brigade to\r\nCairo. I knew General Buell was advancing on Nashville from the\r\nnorth, and I was advised by scouts that the rebels were leaving\r\nthat place, and trying to get out all the supplies they could.\r\nNashville was, at that time, one of the best provisioned posts in\r\nthe South. I had no use for reinforcements now, and thinking Buell\r\nwould like to have his troops again, I ordered Nelson to proceed to\r\nNashville without debarking at Fort Donelson. I sent a gunboat also\r\nas a convoy. The Cumberland River was very high at the time; the\r\nrailroad bridge at Nashville had been burned, and all river craft\r\nhad been destroyed, or would be before the enemy left. Nashville is\r\non the west bank of the Cumberland, and Buell was approaching from\r\nthe east. I thought the steamers carrying Nelson's division would\r\nbe useful in ferrying the balance of Buell's forces across. I\r\nordered Nelson to put himself in communication with Buell as soon\r\nas possible, and if he found him more than two days off from\r\nNashville to return below the city and await orders. Buell,\r\nhowever, had already arrived in person at Edgefield, opposite\r\nNashville, and Mitchell's division of his command reached there the\r\nsame day. Nelson immediately took possession of the city.", 'Paragraph 153': "After Nelson had gone and before I had learned of Buell's\r\narrival, I sent word to department headquarters that I should go to\r\nNashville myself on the 28th if I received no orders to the\r\ncontrary. Hearing nothing, I went as I had informed my superior\r\nofficer I would do. On arriving at Clarksville I saw a fleet of\r\nsteamers at the shore—the same that had taken Nelson's\r\ndivision—and troops going aboard. I landed and called on the\r\ncommanding officer, General C. F. Smith. As soon as he saw me he\r\nshowed an order he had just received from Buell in these words:", 'Paragraph 154': 'NASHVILLE, February 25, 1862.', 'Paragraph 155': 'GENERAL C. F. SMITH,\r\nCommanding U. S. Forces, Clarksville.', 'Paragraph 156': 'GENERAL:—The landing of a portion of our troops, contrary\r\nto my intentions, on the south side of the river has compelled me\r\nto hold this side at every hazard. If the enemy should assume the\r\noffensive, and I am assured by reliable persons that in view of my\r\nposition such is his intention, my force present is altogether\r\ninadequate, consisting of only 15,000 men. I have to request you,\r\ntherefore, to come forward with all the available force under your\r\ncommand. So important do I consider the occasion that I think it\r\nnecessary to give this communication all the force of orders, and I\r\nsend four boats, the Diana, Woodford, John Rain, and Autocrat, to\r\nbring you up. In five or six days my force will probably be\r\nsufficient to relieve you.', 'Paragraph 157': "Very respectfully, your ob't srv't,\r\nD. C. BUELL,\r\nBrigadier-General Comd'g.", 'Paragraph 158': "P. S.—The steamers will leave here at 12 o'clock\r\nto-night.", 'Paragraph 159': 'General Smith said this order was nonsense. But I told him it\r\nwas better to obey it. The General replied, "of course I must\r\nobey," and said his men were embarking as fast as they could. I\r\nwent on up to Nashville and inspected the position taken by\r\nNelson\'s troops. I did not see Buell during the day, and wrote him\r\na note saying that I had been in Nashville since early morning and\r\nhad hoped to meet him. On my return to the boat we met. His troops\r\nwere still east of the river, and the steamers that had carried\r\nNelson\'s division up were mostly at Clarksville to bring Smith\'s\r\ndivision. I said to General Buell my information was that the enemy\r\nwas retreating as fast as possible. General Buell said there was\r\nfighting going on then only ten or twelve miles away. I said:\r\n"Quite probably; Nashville contained valuable stores of arms,\r\nammunition and provisions, and the enemy is probably trying to\r\ncarry away all he can. The fighting is doubtless with the\r\nrear-guard who are trying to protect the trains they are getting\r\naway with." Buell spoke very positively of the danger Nashville was\r\nin of an attack from the enemy. I said, in the absence of positive\r\ninformation, I believed my information was correct. He responded\r\nthat he "knew." "Well," I said, "I do not know; but as I came by\r\nClarksville General Smith\'s troops were embarking to join you."', 'Paragraph 160': "Smith's troops were returned the same day. The enemy were trying\r\nto get away from Nashville and not to return to it.", 'Paragraph 161': 'At this time General Albert Sidney Johnston commanded all the\r\nConfederate troops west of the Alleghany Mountains, with the\r\nexception of those in the extreme south. On the National side the\r\nforces confronting him were divided into, at first three, then four\r\nseparate departments. Johnston had greatly the advantage in having\r\nsupreme command over all troops that could possibly be brought to\r\nbear upon one point, while the forces similarly situated on the\r\nNational side, divided into independent commands, could not be\r\nbrought into harmonious action except by orders from\r\nWashington.', 'Paragraph 162': "At the beginning of 1862 Johnston's troops east of the\r\nMississippi occupied a line extending from Columbus, on his left,\r\nto Mill Springs, on his right. As we have seen, Columbus, both\r\nbanks of the Tennessee River, the west bank of the Cumberland and\r\nBowling Green, all were strongly fortified. Mill Springs was\r\nintrenched. The National troops occupied no territory south of the\r\nOhio, except three small garrisons along its bank and a force\r\nthrown out from Louisville to confront that at Bowling Green.\r\nJohnston's strength was no doubt numerically inferior to that of\r\nthe National troops; but this was compensated for by the advantage\r\nof being sole commander of all the Confederate forces at the West,\r\nand of operating in a country where his friends would take care of\r\nhis rear without any detail of soldiers. But when General George H.\r\nThomas moved upon the enemy at Mill Springs and totally routed him,\r\ninflicting a loss of some 300 killed and wounded, and forts Henry\r\nand Heiman fell into the hands of the National forces, with their\r\narmaments and about 100 prisoners, those losses seemed to\r\ndishearten the Confederate commander so much that he immediately\r\ncommenced a retreat from Bowling Green on Nashville. He reached\r\nthis latter place on the 14th of February, while Donelson was still\r\nbesieged. Buell followed with a portion of the Army of the Ohio,\r\nbut he had to march and did not reach the east bank of the\r\nCumberland opposite Nashville until the 24th of the month, and then\r\nwith only one division of his army.", 'Paragraph 163': "The bridge at Nashville had been destroyed and all boats removed\r\nor disabled, so that a small garrison could have held the place\r\nagainst any National troops that could have been brought against it\r\nwithin ten days after the arrival of the force from Bowling Green.\r\nJohnston seemed to lie quietly at Nashville to await the result at\r\nFort Donelson, on which he had staked the possession of most of the\r\nterritory embraced in the States of Kentucky and Tennessee. It is\r\ntrue, the two generals senior in rank at Fort Donelson were sending\r\nhim encouraging dispatches, even claiming great Confederate\r\nvictories up to the night of the 16th when they must have been\r\npreparing for their individual escape. Johnston made a fatal\r\nmistake in intrusting so important a command to Floyd, who he must\r\nhave known was no soldier even if he possessed the elements of one.\r\nPillow's presence as second was also a mistake. If these officers\r\nhad been forced upon him and designated for that particular\r\ncommand, then he should have left Nashville with a small garrison\r\nunder a trusty officer, and with the remainder of his force gone to\r\nDonelson himself. If he had been captured the result could not have\r\nbeen worse than it was.", 'Paragraph 164': 'Johnston\'s heart failed him upon the first advance of National\r\ntroops. He wrote to Richmond on the 8th of February, "I think the\r\ngunboats of the enemy will probably take Fort Donelson without the\r\nnecessity of employing their land force in cooperation." After the\r\nfall of that place he abandoned Nashville and Chattanooga without\r\nan effort to save either, and fell back into northern Mississippi,\r\nwhere, six weeks later, he was destined to end his career.', 'Paragraph 165': 'From the time of leaving Cairo I was singularly unfortunate in\r\nnot receiving dispatches from General Halleck. The order of the\r\n10th of February directing me to fortify Fort Henry strongly,\r\nparticularly to the land side, and saying that intrenching tools\r\nhad been sent for that purpose, reached me after Donelson was\r\ninvested. I received nothing direct which indicated that the\r\ndepartment commander knew we were in possession of Donelson. I was\r\nreporting regularly to the chief of staff, who had been sent to\r\nCairo, soon after the troops left there, to receive all reports\r\nfrom the front and to telegraph the substance to the St. Louis\r\nheadquarters. Cairo was at the southern end of the telegraph wire.\r\nAnother line was started at once from Cairo to Paducah and\r\nSmithland, at the mouths of the Tennessee and Cumberland\r\nrespectively. My dispatches were all sent to Cairo by boat, but\r\nmany of those addressed to me were sent to the operator at the end\r\nof the advancing wire and he failed to forward them. This operator\r\nafterwards proved to be a rebel; he deserted his post after a short\r\ntime and went south taking his dispatches with him. A telegram from\r\nGeneral McClellan to me of February 16th, the day of the surrender,\r\ndirecting me to report in full the situation, was not received at\r\nmy headquarters until the 3d of March.', 'Paragraph 166': 'On the 2d of March I received orders dated March 1st to move my\r\ncommand back to Fort Henry, leaving only a small garrison at\r\nDonelson. From Fort Henry expeditions were to be sent against\r\nEastport, Mississippi, and Paris, Tennessee. We started from\r\nDonelson on the 4th, and the same day I was back on the Tennessee\r\nRiver. On March 4th I also received the following dispatch from\r\nGeneral Halleck:', 'Paragraph 167': 'MAJ.-GEN. U. S. GRANT,\r\nFort Henry:', 'Paragraph 168': 'You will place Maj.-Gen. C. F. Smith in command of expedition,\r\nand remain yourself at Fort Henry. Why do you not obey my orders to\r\nreport strength and positions of your command?', 'Paragraph 169': 'H. W. HALLECK,\r\nMajor-General.', 'Paragraph 170': 'I was surprised. This was the first intimation I had received\r\nthat General Halleck had called for information as to the strength\r\nof my command. On the 6th he wrote to me again. "Your going to\r\nNashville without authority, and when your presence with your\r\ntroops was of the utmost importance, was a matter of very serious\r\ncomplaint at Washington, so much so that I was advised to arrest\r\nyou on your return." This was the first I knew of his objecting to\r\nmy going to Nashville. That place was not beyond the limits of my\r\ncommand, which, it had been expressly declared in orders, were "not\r\ndefined." Nashville is west of the Cumberland River, and I had sent\r\ntroops that had reported to me for duty to occupy the place. I\r\nturned over the command as directed and then replied to General\r\nHalleck courteously, but asked to be relieved from further duty\r\nunder him.', 'Paragraph 171': 'Later I learned that General Halleck had been calling lustily\r\nfor more troops, promising that he would do something important if\r\nhe could only be sufficiently reinforced. McClellan asked him what\r\nforce he then had. Halleck telegraphed me to supply the information\r\nso far as my command was concerned, but I received none of his\r\ndispatches. At last Halleck reported to Washington that he had\r\nrepeatedly ordered me to give the strength of my force, but could\r\nget nothing out of me; that I had gone to Nashville, beyond the\r\nlimits of my command, without his authority, and that my army was\r\nmore demoralized by victory than the army at Bull Run had been by\r\ndefeat. General McClellan, on this information, ordered that I\r\nshould be relieved from duty and that an investigation should be\r\nmade into any charges against me. He even authorized my arrest.\r\nThus in less than two weeks after the victory at Donelson, the two\r\nleading generals in the army were in correspondence as to what\r\ndisposition should be made of me, and in less than three weeks I\r\nwas virtually in arrest and without a command.', 'Paragraph 172': 'On the 13th of March I was restored to command, and on the 17th\r\nHalleck sent me a copy of an order from the War Department which\r\nstated that accounts of my misbehavior had reached Washington and\r\ndirected him to investigate and report the facts. He forwarded also\r\na copy of a detailed dispatch from himself to Washington entirely\r\nexonerating me; but he did not inform me that it was his own\r\nreports that had created all the trouble. On the contrary, he wrote\r\nto me, "Instead of relieving you, I wish you, as soon as your new\r\narmy is in the field, to assume immediate command, and lead it to\r\nnew victories." In consequence I felt very grateful to him, and\r\nsupposed it was his interposition that had set me right with the\r\ngovernment. I never knew the truth until General Badeau unearthed\r\nthe facts in his researches for his history of my campaigns.', 'Paragraph 173': "General Halleck unquestionably deemed General C. F. Smith a much\r\nfitter officer for the command of all the forces in the military\r\ndistrict than I was, and, to render him available for such command,\r\ndesired his promotion to antedate mine and those of the other\r\ndivision commanders. It is probable that the general opinion was\r\nthat Smith's long services in the army and distinguished deeds\r\nrendered him the more proper person for such command. Indeed I was\r\nrather inclined to this opinion myself at that time, and would have\r\nserved as faithfully under Smith as he had done under me. But this\r\ndid not justify the dispatches which General Halleck sent to\r\nWashington, or his subsequent concealment of them from me when\r\npretending to explain the action of my superiors.", 'Paragraph 174': 'On receipt of the order restoring me to command I proceeded to\r\nSavannah on the Tennessee, to which point my troops had advanced.\r\nGeneral Smith was delighted to see me and was unhesitating in his\r\ndenunciation of the treatment I had received. He was on a sick bed\r\nat the time, from which he never came away alive. His death was a\r\nsevere loss to our western army. His personal courage was\r\nunquestioned, his judgment and professional acquirements were\r\nunsurpassed, and he had the confidence of those he commanded as\r\nwell as of those over him.', 'Paragraph 175': "When I reassumed command on the 17th of March I found the army\r\ndivided, about half being on the east bank of the Tennessee at\r\nSavannah, while one division was at Crump's landing on the west\r\nbank about four miles higher up, and the remainder at Pittsburg\r\nlanding, five miles above Crump's. The enemy was in force at\r\nCorinth, the junction of the two most important railroads in the\r\nMississippi valley—one connecting Memphis and the Mississippi\r\nRiver with the East, and the other leading south to all the cotton\r\nstates. Still another railroad connects Corinth with Jackson, in\r\nwest Tennessee. If we obtained possession of Corinth the enemy\r\nwould have no railroad for the transportation of armies or supplies\r\nuntil that running east from Vicksburg was reached. It was the\r\ngreat strategic position at the West between the Tennessee and the\r\nMississippi rivers and between Nashville and Vicksburg.", 'Paragraph 176': 'I at once put all the troops at Savannah in motion for Pittsburg\r\nlanding, knowing that the enemy was fortifying at Corinth and\r\ncollecting an army there under Johnston. It was my expectation to\r\nmarch against that army as soon as Buell, who had been ordered to\r\nreinforce me with the Army of the Ohio, should arrive; and the west\r\nbank of the river was the place to start from. Pittsburg is only\r\nabout twenty miles from Corinth, and Hamburg landing, four miles\r\nfurther up the river, is a mile or two nearer. I had not been in\r\ncommand long before I selected Hamburg as the place to put the Army\r\nof the Ohio when it arrived. The roads from Pittsburg and Hamburg\r\nto Corinth converge some eight miles out. This disposition of the\r\ntroops would have given additional roads to march over when the\r\nadvance commenced, within supporting distance of each other.', 'Paragraph 177': 'Before I arrived at Savannah, Sherman, who had joined the Army\r\nof the Tennessee and been placed in command of a division, had made\r\nan expedition on steamers convoyed by gunboats to the neighborhood\r\nof Eastport, thirty miles south, for the purpose of destroying the\r\nrailroad east of Corinth. The rains had been so heavy for some time\r\nbefore that the low-lands had become impassable swamps. Sherman\r\ndebarked his troops and started out to accomplish the object of the\r\nexpedition; but the river was rising so rapidly that the back-water\r\nup the small tributaries threatened to cut off the possibility of\r\ngetting back to the boats, and the expedition had to return without\r\nreaching the railroad. The guns had to be hauled by hand through\r\nthe water to get back to the boats.', 'Paragraph 178': "On the 17th of March the army on the Tennessee River consisted\r\nof five divisions, commanded respectively by Generals C. F. Smith,\r\nMcClernand, L. Wallace, Hurlbut and Sherman. General W. H. L.\r\nWallace was temporarily in command of Smith's division, General\r\nSmith, as I have said, being confined to his bed. Reinforcements\r\nwere arriving daily and as they came up they were organized, first\r\ninto brigades, then into a division, and the command given to\r\nGeneral Prentiss, who had been ordered to report to me. General\r\nBuell was on his way from Nashville with 40,000 veterans. On the\r\n19th of March he was at Columbia, Tennessee, eighty-five miles from\r\nPittsburg. When all reinforcements should have arrived I expected\r\nto take the initiative by marching on Corinth, and had no\r\nexpectation of needing fortifications, though this subject was\r\ntaken into consideration. McPherson, my only military engineer, was\r\ndirected to lay out a line to intrench. He did so, but reported\r\nthat it would have to be made in rear of the line of encampment as\r\nit then ran. The new line, while it would be nearer the river, was\r\nyet too far away from the Tennessee, or even from the creeks, to be\r\neasily supplied with water, and in case of attack these creeks\r\nwould be in the hands of the enemy. The fact is, I regarded the\r\ncampaign we were engaged in as an offensive one and had no idea\r\nthat the enemy would leave strong intrenchments to take the\r\ninitiative when he knew he would be attacked where he was if he\r\nremained. This view, however, did not prevent every precaution\r\nbeing taken and every effort made to keep advised of all movements\r\nof the enemy.", 'Paragraph 179': "Johnston's cavalry meanwhile had been well out towards our\r\nfront, and occasional encounters occurred between it and our\r\noutposts. On the 1st of April this cavalry became bold and\r\napproached our lines, showing that an advance of some kind was\r\ncontemplated. On the 2d Johnston left Corinth in force to attack my\r\narmy. On the 4th his cavalry dashed down and captured a small\r\npicket guard of six or seven men, stationed some five miles out\r\nfrom Pittsburg on the Corinth road. Colonel Buckland sent relief to\r\nthe guard at once and soon followed in person with an entire\r\nregiment, and General Sherman followed Buckland taking the\r\nremainder of a brigade. The pursuit was kept up for some three\r\nmiles beyond the point where the picket guard had been captured,\r\nand after nightfall Sherman returned to camp and reported to me by\r\nletter what had occurred.", 'Paragraph 180': "At this time a large body of the enemy was hovering to the west\r\nof us, along the line of the Mobile and Ohio railroad. My\r\napprehension was much greater for the safety of Crump's landing\r\nthan it was for Pittsburg. I had no apprehension that the enemy\r\ncould really capture either place. But I feared it was possible\r\nthat he might make a rapid dash upon Crump's and destroy our\r\ntransports and stores, most of which were kept at that point, and\r\nthen retreat before Wallace could be reinforced. Lew. Wallace's\r\nposition I regarded as so well chosen that he was not removed.", 'Paragraph 181': 'At this time I generally spent the day at Pittsburg and returned\r\nto Savannah in the evening. I was intending to remove my\r\nheadquarters to Pittsburg, but Buell was expected daily and would\r\ncome in at Savannah. I remained at this point, therefore, a few\r\ndays longer than I otherwise should have done, in order to meet him\r\non his arrival. The skirmishing in our front, however, had been so\r\ncontinuous from about the 3d of April that I did not leave\r\nPittsburg each night until an hour when I felt there would be no\r\nfurther danger before the morning.', 'Paragraph 182': "On Friday the 4th, the day of Buckland's advance, I was very\r\nmuch injured by my horse falling with me, and on me, while I was\r\ntrying to get to the front where firing had been heard. The night\r\nwas one of impenetrable darkness, with rain pouring down in\r\ntorrents; nothing was visible to the eye except as revealed by the\r\nfrequent flashes of lightning. Under these circumstances I had to\r\ntrust to the horse, without guidance, to keep the road. I had not\r\ngone far, however, when I met General W. H. L. Wallace and Colonel\r\n(afterwards General) McPherson coming from the direction of the\r\nfront. They said all was quiet so far as the enemy was concerned.\r\nOn the way back to the boat my horse's feet slipped from under him,\r\nand he fell with my leg under his body. The extreme softness of the\r\nground, from the excessive rains of the few preceding days, no\r\ndoubt saved me from a severe injury and protracted lameness. As it\r\nwas, my ankle was very much injured, so much so that my boot had to\r\nbe cut off. For two or three days after I was unable to walk except\r\nwith crutches.", 'Paragraph 183': "On the 5th General Nelson, with a division of Buell's army,\r\narrived at Savannah and I ordered him to move up the east bank of\r\nthe river, to be in a position where he could be ferried over to\r\nCrump's landing or Pittsburg as occasion required. I had learned\r\nthat General Buell himself would be at Savannah the next day, and\r\ndesired to meet me on his arrival. Affairs at Pittsburg landing had\r\nbeen such for several days that I did not want to be away during\r\nthe day. I determined, therefore, to take a very early breakfast\r\nand ride out to meet Buell, and thus save time. He had arrived on\r\nthe evening of the 5th, but had not advised me of the fact and I\r\nwas not aware of it until some time after. While I was at\r\nbreakfast, however, heavy firing was heard in the direction of\r\nPittsburg landing, and I hastened there, sending a hurried note to\r\nBuell informing him of the reason why I could not meet him at\r\nSavannah. On the way up the river I directed the dispatch-boat to\r\nrun in close to Crump's landing, so that I could communicate with\r\nGeneral Lew. Wallace. I found him waiting on a boat apparently\r\nexpecting to see me, and I directed him to get his troops in line\r\nready to execute any orders he might receive. He replied that his\r\ntroops were already under arms and prepared to move.", 'Paragraph 184': "Up to that time I had felt by no means certain that Crump's\r\nlanding might not be the point of attack. On reaching the front,\r\nhowever, about eight A.M., I found that the attack on Pittsburg was\r\nunmistakable, and that nothing more than a small guard, to protect\r\nour transports and stores, was needed at Crump's. Captain Baxter, a\r\nquartermaster on my staff, was accordingly directed to go back and\r\norder General Wallace to march immediately to Pittsburg by the road\r\nnearest the river. Captain Baxter made a memorandum of this order.\r\nAbout one P.M., not hearing from Wallace and being much in need of\r\nreinforcements, I sent two more of my staff, Colonel McPherson and\r\nCaptain Rowley, to bring him up with his division. They reported\r\nfinding him marching towards Purdy, Bethel, or some point west from\r\nthe river, and farther from Pittsburg by several miles than when he\r\nstarted. The road from his first position to Pittsburg landing was\r\ndirect and near the river. Between the two points a bridge had been\r\nbuilt across Snake Creek by our troops, at which Wallace's command\r\nhad assisted, expressly to enable the troops at the two places to\r\nsupport each other in case of need. Wallace did not arrive in time\r\nto take part in the first day's fight. General Wallace has since\r\nclaimed that the order delivered to him by Captain Baxter was\r\nsimply to join the right of the army, and that the road over which\r\nhe marched would have taken him to the road from Pittsburg to Purdy\r\nwhere it crosses Owl Creek on the right of Sherman; but this is not\r\nwhere I had ordered him nor where I wanted him to go.", 'Paragraph 185': 'I never could see and do not now see why any order was necessary\r\nfurther than to direct him to come to Pittsburg landing, without\r\nspecifying by what route. His was one of three veteran divisions\r\nthat had been in battle, and its absence was severely felt. Later\r\nin the war General Wallace would not have made the mistake that he\r\ncommitted on the 6th of April, 1862. I presume his idea was that by\r\ntaking the route he did he would be able to come around on the\r\nflank or rear of the enemy, and thus perform an act of heroism that\r\nwould redound to the credit of his command, as well as to the\r\nbenefit of his country.', 'Paragraph 186': "Some two or three miles from Pittsburg landing was a log\r\nmeeting-house called Shiloh. It stood on the ridge which divides\r\nthe waters of Snake and Lick creeks, the former emptying into the\r\nTennessee just north of Pittsburg landing, and the latter south.\r\nThis point was the key to our position and was held by Sherman. His\r\ndivision was at that time wholly raw, no part of it ever having\r\nbeen in an engagement; but I thought this deficiency was more than\r\nmade up by the superiority of the commander. McClernand was on\r\nSherman's left, with troops that had been engaged at forts Henry\r\nand Donelson and were therefore veterans so far as western troops\r\nhad become such at that stage of the war. Next to McClernand came\r\nPrentiss with a raw division, and on the extreme left, Stuart with\r\none brigade of Sherman's division. Hurlbut was in rear of Prentiss,\r\nmassed, and in reserve at the time of the onset. The division of\r\nGeneral C. F. Smith was on the right, also in reserve. General\r\nSmith was still sick in bed at Savannah, but within hearing of our\r\nguns. His services would no doubt have been of inestimable value\r\nhad his health permitted his presence. The command of his division\r\ndevolved upon Brigadier-General W. H. L. Wallace, a most estimable\r\nand able officer; a veteran too, for he had served a year in the\r\nMexican war and had been with his command at Henry and Donelson.\r\nWallace was mortally wounded in the first day's engagement, and\r\nwith the change of commanders thus necessarily effected in the heat\r\nof battle the efficiency of his division was much weakened.", 'Paragraph 187': 'The position of our troops made a continuous line from Lick\r\nCreek on the left to Owl Creek, a branch of Snake Creek, on the\r\nright, facing nearly south and possibly a little west. The water in\r\nall these streams was very high at the time and contributed to\r\nprotect our flanks. The enemy was compelled, therefore, to attack\r\ndirectly in front. This he did with great vigor, inflicting heavy\r\nlosses on the National side, but suffering much heavier on his\r\nown.', 'Paragraph 188': 'The Confederate assaults were made with such a disregard of\r\nlosses on their own side that our line of tents soon fell into\r\ntheir hands. The ground on which the battle was fought was\r\nundulating, heavily timbered with scattered clearings, the woods\r\ngiving some protection to the troops on both sides. There was also\r\nconsiderable underbrush. A number of attempts were made by the\r\nenemy to turn our right flank, where Sherman was posted, but every\r\neffort was repulsed with heavy loss. But the front attack was kept\r\nup so vigorously that, to prevent the success of these attempts to\r\nget on our flanks, the National troops were compelled, several\r\ntimes, to take positions to the rear nearer Pittsburg landing. When\r\nthe firing ceased at night the National line was all of a mile in\r\nrear of the position it had occupied in the morning.', 'Paragraph 189': "In one of the backward moves, on the 6th, the division commanded\r\nby General Prentiss did not fall back with the others. This left\r\nhis flanks exposed and enabled the enemy to capture him with about\r\n2,200 of his officers and men. General Badeau gives four o'clock of\r\nthe 6th as about the time this capture took place. He may be right\r\nas to the time, but my recollection is that the hour was later.\r\nGeneral Prentiss himself gave the hour as half-past five. I was\r\nwith him, as I was with each of the division commanders that day,\r\nseveral times, and my recollection is that the last time I was with\r\nhim was about half-past four, when his division was standing up\r\nfirmly and the General was as cool as if expecting victory. But no\r\nmatter whether it was four or later, the story that he and his\r\ncommand were surprised and captured in their camps is without any\r\nfoundation whatever. If it had been true, as currently reported at\r\nthe time and yet believed by thousands of people, that Prentiss and\r\nhis division had been captured in their beds, there would not have\r\nbeen an all-day struggle, with the loss of thousands killed and\r\nwounded on the Confederate side.", 'Paragraph 190': 'With the single exception of a few minutes after the capture of\r\nPrentiss, a continuous and unbroken line was maintained all day\r\nfrom Snake Creek or its tributaries on the right to Lick Creek or\r\nthe Tennessee on the left above Pittsburg.', 'Paragraph 191': "There was no hour during the day when there was not heavy firing\r\nand generally hard fighting at some point on the line, but seldom\r\nat all points at the same time. It was a case of Southern dash\r\nagainst Northern pluck and endurance. Three of the five divisions\r\nengaged on Sunday were entirely raw, and many of the men had only\r\nreceived their arms on the way from their States to the field. Many\r\nof them had arrived but a day or two before and were hardly able to\r\nload their muskets according to the manual. Their officers were\r\nequally ignorant of their duties. Under these circumstances it is\r\nnot astonishing that many of the regiments broke at the first fire.\r\nIn two cases, as I now remember, colonels led their regiments from\r\nthe field on first hearing the whistle of the enemy's bullets. In\r\nthese cases the colonels were constitutional cowards, unfit for any\r\nmilitary position; but not so the officers and men led out of\r\ndanger by them. Better troops never went upon a battle-field than\r\nmany of these, officers and men, afterwards proved themselves to\r\nbe, who fled panic stricken at the first whistle of bullets and\r\nshell at Shiloh.", 'Paragraph 192': 'During the whole of Sunday I was continuously engaged in passing\r\nfrom one part of the field to another, giving directions to\r\ndivision commanders. In thus moving along the line, however, I\r\nnever deemed it important to stay long with Sherman. Although his\r\ntroops were then under fire for the first time, their commander, by\r\nhis constant presence with them, inspired a confidence in officers\r\nand men that enabled them to render services on that bloody\r\nbattle-field worthy of the best of veterans. McClernand was next to\r\nSherman, and the hardest fighting was in front of these two\r\ndivisions. McClernand told me on that day, the 6th, that he\r\nprofited much by having so able a commander supporting him. A\r\ncasualty to Sherman that would have taken him from the field that\r\nday would have been a sad one for the troops engaged at Shiloh. And\r\nhow near we came to this! On the 6th Sherman was shot twice, once\r\nin the hand, once in the shoulder, the ball cutting his coat and\r\nmaking a slight wound, and a third ball passed through his hat. In\r\naddition to this he had several horses shot during the day.', 'Paragraph 193': 'The nature of this battle was such that cavalry could not be\r\nused in front; I therefore formed ours into line in rear, to stop\r\nstragglers—of whom there were many. When there would be\r\nenough of them to make a show, and after they had recovered from\r\ntheir fright, they would be sent to reinforce some part of the line\r\nwhich needed support, without regard to their companies, regiments\r\nor brigades.', 'Paragraph 194': "On one occasion during the day I rode back as far as the river\r\nand met General Buell, who had just arrived; I do not remember the\r\nhour, but at that time there probably were as many as four or five\r\nthousand stragglers lying under cover of the river bluff,\r\npanic-stricken, most of whom would have been shot where they lay,\r\nwithout resistance, before they would have taken muskets and\r\nmarched to the front to protect themselves. This meeting between\r\nGeneral Buell and myself was on the dispatch-boat used to run\r\nbetween the landing and Savannah. It was brief, and related\r\nspecially to his getting his troops over the river. As we left the\r\nboat together, Buell's attention was attracted by the men lying\r\nunder cover of the river bank. I saw him berating them and trying\r\nto shame them into joining their regiments. He even threatened them\r\nwith shells from the gunboats near by. But it was all to no effect.\r\nMost of these men afterward proved themselves as gallant as any of\r\nthose who saved the battle from which they had deserted. I have no\r\ndoubt that this sight impressed General Buell with the idea that a\r\nline of retreat would be a good thing just then. If he had come in\r\nby the front instead of through the stragglers in the rear, he\r\nwould have thought and felt differently. Could he have come through\r\nthe Confederate rear, he would have witnessed there a scene similar\r\nto that at our own. The distant rear of an army engaged in battle\r\nis not the best place from which to judge correctly what is going\r\non in front. Later in the war, while occupying the country between\r\nthe Tennessee and the Mississippi, I learned that the panic in the\r\nConfederate lines had not differed much from that within our own.\r\nSome of the country people estimated the stragglers from Johnston's\r\narmy as high as 20,000. Of course this was an exaggeration.", 'Paragraph 195': "The situation at the close of Sunday was as follows: along the\r\ntop of the bluff just south of the log-house which stood at\r\nPittsburg landing, Colonel J. D. Webster, of my staff, had arranged\r\ntwenty or more pieces of artillery facing south or up the river.\r\nThis line of artillery was on the crest of a hill overlooking a\r\ndeep ravine opening into the Tennessee. Hurlbut with his division\r\nintact was on the right of this artillery, extending west and\r\npossibly a little north. McClernand came next in the general line,\r\nlooking more to the west. His division was complete in its\r\norganization and ready for any duty. Sherman came next, his right\r\nextending to Snake Creek. His command, like the other two, was\r\ncomplete in its organization and ready, like its chief, for any\r\nservice it might be called upon to render. All three divisions\r\nwere, as a matter of course, more or less shattered and depleted in\r\nnumbers from the terrible battle of the day. The division of W. H.\r\nL. Wallace, as much from the disorder arising from changes of\r\ndivision and brigade commanders, under heavy fire, as from any\r\nother cause, had lost its organization and did not occupy a place\r\nin the line as a division. Prentiss' command was gone as a\r\ndivision, many of its members having been killed, wounded or\r\ncaptured, but it had rendered valiant services before its final\r\ndispersal, and had contributed a good share to the defence of\r\nShiloh.", 'Paragraph 196': "The right of my line rested near the bank of Snake Creek, a\r\nshort distance above the bridge which had been built by the troops\r\nfor the purpose of connecting Crump's landing and Pittsburg\r\nlanding. Sherman had posted some troops in a log-house and\r\nout-buildings which overlooked both the bridge over which Wallace\r\nwas expected and the creek above that point. In this last position\r\nSherman was frequently attacked before night, but held the point\r\nuntil he voluntarily abandoned it to advance in order to make room\r\nfor Lew. Wallace, who came up after dark.", 'Paragraph 197': "There was, as I have said, a deep ravine in front of our left.\r\nThe Tennessee River was very high and there was water to a\r\nconsiderable depth in the ravine. Here the enemy made a last\r\ndesperate effort to turn our flank, but was repelled. The gunboats\r\nTyler and Lexington, Gwin and Shirk commanding, with the artillery\r\nunder Webster, aided the army and effectually checked their further\r\nprogress. Before any of Buell's troops had reached the west bank of\r\nthe Tennessee, firing had almost entirely ceased; anything like an\r\nattempt on the part of the enemy to advance had absolutely ceased.\r\nThere was some artillery firing from an unseen enemy, some of his\r\nshells passing beyond us; but I do not remember that there was the\r\nwhistle of a single musket-ball heard. As his troops arrived in the\r\ndusk General Buell marched several of his regiments part way down\r\nthe face of the hill where they fired briskly for some minutes, but\r\nI do not think a single man engaged in this firing received an\r\ninjury. The attack had spent its force.", 'Paragraph 198': "General Lew. Wallace, with 5,000 effective men, arrived after\r\nfiring had ceased for the day, and was placed on the right. Thus\r\nnight came, Wallace came, and the advance of Nelson's division\r\ncame; but none—unless night—in time to be of material\r\nservice to the gallant men who saved Shiloh on that first day\r\nagainst large odds. Buell's loss on the 6th of April was two men\r\nkilled and one wounded, all members of the 36th Indiana infantry.\r\nThe Army of the Tennessee lost on that day at least 7,000 men. The\r\npresence of two or three regiments of Buell's army on the west bank\r\nbefore firing ceased had not the slightest effect in preventing the\r\ncapture of Pittsburg landing.", 'Paragraph 199': 'So confident was I before firing had ceased on the 6th that the\r\nnext day would bring victory to our arms if we could only take the\r\ninitiative, that I visited each division commander in person before\r\nany reinforcements had reached the field. I directed them to throw\r\nout heavy lines of skirmishers in the morning as soon as they could\r\nsee, and push them forward until they found the enemy, following\r\nwith their entire divisions in supporting distance, and to engage\r\nthe enemy as soon as found. To Sherman I told the story of the\r\nassault at Fort Donelson, and said that the same tactics would win\r\nat Shiloh. Victory was assured when Wallace arrived, even if there\r\nhad been no other support. I was glad, however, to see the\r\nreinforcements of Buell and credit them with doing all there was\r\nfor them to do.', 'Paragraph 200': "During the night of the 6th the remainder of Nelson's division,\r\nBuell's army crossed the river and were ready to advance in the\r\nmorning, forming the left wing. Two other divisions, Crittenden's\r\nand McCook's, came up the river from Savannah in the transports and\r\nwere on the west bank early on the 7th. Buell commanded them in\r\nperson. My command was thus nearly doubled in numbers and\r\nefficiency.", 'Paragraph 201': 'During the night rain fell in torrents and our troops were\r\nexposed to the storm without shelter. I made my headquarters under\r\na tree a few hundred yards back from the river bank. My ankle was\r\nso much swollen from the fall of my horse the Friday night\r\npreceding, and the bruise was so painful, that I could get no\r\nrest.', 'Paragraph 202': "The drenching rain would have precluded the possibility of sleep\r\nwithout this additional cause. Some time after midnight, growing\r\nrestive under the storm and the continuous pain, I moved back to\r\nthe log-house under the bank. This had been taken as a hospital,\r\nand all night wounded men were being brought in, their wounds\r\ndressed, a leg or an arm amputated as the case might require, and\r\neverything being done to save life or alleviate suffering. The\r\nsight was more unendurable than encountering the enemy's fire, and\r\nI returned to my tree in the rain.", 'Paragraph 203': "The advance on the morning of the 7th developed the enemy in the\r\ncamps occupied by our troops before the battle began, more than a\r\nmile back from the most advanced position of the Confederates on\r\nthe day before. It is known now that they had not yet learned of\r\nthe arrival of Buell's command. Possibly they fell back so far to\r\nget the shelter of our tents during the rain, and also to get away\r\nfrom the shells that were dropped upon them by the gunboats every\r\nfifteen minutes during the night.", 'Paragraph 204': "The position of the Union troops on the morning of the 7th was\r\nas follows: General Lew. Wallace on the right; Sherman on his left;\r\nthen McClernand and then Hurlbut. Nelson, of Buell's army, was on\r\nour extreme left, next to the river.", 'Paragraph 205': "Crittenden was next in line after Nelson and on his right,\r\nMcCook followed and formed the extreme right of Buell's command. My\r\nold command thus formed the right wing, while the troops directly\r\nunder Buell constituted the left wing of the army. These relative\r\npositions were retained during the entire day, or until the enemy\r\nwas driven from the field.", 'Paragraph 206': "In a very short time the battle became general all along the\r\nline. This day everything was favorable to the Union side. We had\r\nnow become the attacking party. The enemy was driven back all day,\r\nas we had been the day before, until finally he beat a precipitate\r\nretreat. The last point held by him was near the road leading from\r\nthe landing to Corinth, on the left of Sherman and right of\r\nMcClernand. About three o'clock, being near that point and seeing\r\nthat the enemy was giving way everywhere else, I gathered up a\r\ncouple of regiments, or parts of regiments, from troops near by,\r\nformed them in line of battle and marched them forward, going in\r\nfront myself to prevent premature or long-range firing. At this\r\npoint there was a clearing between us and the enemy favorable for\r\ncharging, although exposed. I knew the enemy were ready to break\r\nand only wanted a little encouragement from us to go quickly and\r\njoin their friends who had started earlier. After marching to\r\nwithin musket-range I stopped and let the troops pass. The command,\r\nCHARGE, was given, and was executed with loud cheers and with a\r\nrun; when the last of the enemy broke.", 'Paragraph 207': '\r\n[NOTE.—Since writing this chapter I have received from Mrs.\r\nW. H. L. Wallace, widow of the gallant general who was killed in\r\nthe first day\'s fight on the field of Shiloh, a letter from General\r\nLew. Wallace to him dated the morning of the 5th. At the date of\r\nthis letter it was well known that the Confederates had troops out\r\nalong the Mobile & Ohio railroad west of Crump\'s landing and\r\nPittsburg landing, and were also collecting near Shiloh. This\r\nletter shows that at that time General Lew. Wallace was making\r\npreparations for the emergency that might happen for the passing of\r\nreinforcements between Shiloh and his position, extending from\r\nCrump\'s landing westward, and he sends it over the road running\r\nfrom Adamsville to the Pittsburg landing and Purdy road. These two\r\nroads intersect nearly a mile west of the crossing of the latter\r\nover Owl Creek, where our right rested. In this letter General Lew.\r\nWallace advises General W. H. L. Wallace that he will send\r\n"to-morrow" (and his letter also says "April 5th," which is the\r\nsame day the letter was dated and which, therefore, must have been\r\nwritten on the 4th) some cavalry to report to him at his\r\nheadquarters, and suggesting the propriety of General W. H. L.\r\nWallace\'s sending a company back with them for the purpose of\r\nhaving the cavalry at the two landings familiarize themselves with\r\nthe road so that they could "act promptly in case of emergency as\r\nguides to and from the different camps."\n\r\nThis modifies very materially what I have said, and what has been\r\nsaid by others, of the conduct of General Lew. Wallace at the\r\nbattle of Shiloh. It shows that he naturally, with no more\r\nexperience than he had at the time in the profession of arms, would\r\ntake the particular road that he did start upon in the absence of\r\norders to move by a different road.\n\r\nThe mistake he made, and which probably caused his apparent\r\ndilatoriness, was that of advancing some distance after he found\r\nthat the firing, which would be at first directly to his front and\r\nthen off to the left, had fallen back until it had got very much in\r\nrear of the position of his advance. This falling back had taken\r\nplace before I sent General Wallace orders to move up to Pittsburg\r\nlanding and, naturally, my order was to follow the road nearest the\r\nriver. But my order was verbal, and to a staff officer who was to\r\ndeliver it to General Wallace, so that I am not competent to say\r\njust what order the General actually received.\n\r\nGeneral Wallace\'s division was stationed, the First brigade at\r\nCrump\'s landing, the Second out two miles, and the Third two and a\r\nhalf miles out. Hearing the sounds of battle General Wallace early\r\nordered his First and Third brigades to concentrate on the Second.\r\nIf the position of our front had not changed, the road which\r\nWallace took would have been somewhat shorter to our right than the\r\nRiver road.\n\r\nU. S. GRANT.\n\r\nMOUNT MACGREGOR, NEW YORK, June 21, 1885.', 'Paragraph 208': "During this second day of the battle I had been moving from\r\nright to left and back, to see for myself the progress made. In the\r\nearly part of the afternoon, while riding with Colonel McPherson\r\nand Major Hawkins, then my chief commissary, we got beyond the left\r\nof our troops. We were moving along the northern edge of a\r\nclearing, very leisurely, toward the river above the landing. There\r\ndid not appear to be an enemy to our right, until suddenly a\r\nbattery with musketry opened upon us from the edge of the woods on\r\nthe other side of the clearing. The shells and balls whistled about\r\nour ears very fast for about a minute. I do not think it took us\r\nlonger than that to get out of range and out of sight. In the\r\nsudden start we made, Major Hawkins lost his hat. He did not stop\r\nto pick it up. When we arrived at a perfectly safe position we\r\nhalted to take an account of damages. McPherson's horse was panting\r\nas if ready to drop. On examination it was found that a ball had\r\nstruck him forward of the flank just back of the saddle, and had\r\ngone entirely through. In a few minutes the poor beast dropped\r\ndead; he had given no sign of injury until we came to a stop. A\r\nball had struck the metal scabbard of my sword, just below the\r\nhilt, and broken it nearly off; before the battle was over it had\r\nbroken off entirely. There were three of us: one had lost a horse,\r\nkilled; one a hat and one a sword-scabbard. All were thankful that\r\nit was no worse.", 'Paragraph 209': 'After the rain of the night before and the frequent and heavy\r\nrains for some days previous, the roads were almost impassable. The\r\nenemy carrying his artillery and supply trains over them in his\r\nretreat, made them still worse for troops following. I wanted to\r\npursue, but had not the heart to order the men who had fought\r\ndesperately for two days, lying in the mud and rain whenever not\r\nfighting, and I did not feel disposed to positively order Buell, or\r\nany part of his command, to pursue. Although the senior in rank at\r\nthe time I had been so only a few weeks. Buell was, and had been\r\nfor some time past, a department commander, while I commanded only\r\na district. I did not meet Buell in person until too late to get\r\ntroops ready and pursue with effect; but had I seen him at the\r\nmoment of the last charge I should have at least requested him to\r\nfollow.', 'Paragraph 210': "[NOTE: In an article on the battle of Shiloh which I\r\nwrote for the Century Magazine, I stated that General A. McD.\r\nMcCook, who commanded a division of Buell's army, expressed some\r\nunwillingness to pursue the enemy on Monday, April 7th, because of\r\nthe condition of his troops. General Badeau, in his history, also\r\nmakes the same statement, on my authority. Out of justice to\r\nGeneral McCook and his command, I must say that they left a point\r\ntwenty-two miles east of Savannah on the morning of the 6th. From\r\nthe heavy rains of a few days previous and the passage of trains\r\nand artillery, the roads were necessarily deep in mud, which made\r\nmarching slow. The division had not only marched through this mud\r\nthe day before, but it had been in the rain all night without rest.\r\nIt was engaged in the battle of the second day and did as good\r\nservice as its position allowed. In fact an opportunity occurred\r\nfor it to perform a conspicuous act of gallantry which elicited the\r\nhighest commendation from division commanders in the Army of the\r\nTennessee. General Sherman both in his memoirs and report makes\r\nmention of this fact. General McCook himself belongs to a family\r\nwhich furnished many volunteers to the army. I refer to these\r\ncircumstances with minuteness because I did General McCook\r\ninjustice in my article in the Century, though not to the extent\r\none would suppose from the public press. I am not willing to do any\r\none an injustice, and if convinced that I have done one, I am\r\nalways willing to make the fullest admission.]", 'Paragraph 211': 'I rode forward several miles the day after the battle, and found\r\nthat the enemy had dropped much, if not all, of their provisions,\r\nsome ammunition and the extra wheels of their caissons, lightening\r\ntheir loads to enable them to get off their guns. About five miles\r\nout we found their field hospital abandoned. An immediate pursuit\r\nmust have resulted in the capture of a considerable number of\r\nprisoners and probably some guns.', 'Paragraph 212': 'Shiloh was the severest battle fought at the West during the\r\nwar, and but few in the East equalled it for hard, determined\r\nfighting. I saw an open field, in our possession on the second day,\r\nover which the Confederates had made repeated charges the day\r\nbefore, so covered with dead that it would have been possible to\r\nwalk across the clearing, in any direction, stepping on dead\r\nbodies, without a foot touching the ground. On our side National\r\nand Confederate troops were mingled together in about equal\r\nproportions; but on the remainder of the field nearly all were\r\nConfederates. On one part, which had evidently not been ploughed\r\nfor several years, probably because the land was poor, bushes had\r\ngrown up, some to the height of eight or ten feet. There was not\r\none of these left standing unpierced by bullets. The smaller ones\r\nwere all cut down.', 'Paragraph 213': 'Contrary to all my experience up to that time, and to the\r\nexperience of the army I was then commanding, we were on the\r\ndefensive. We were without intrenchments or defensive advantages of\r\nany sort, and more than half the army engaged the first day was\r\nwithout experience or even drill as soldiers. The officers with\r\nthem, except the division commanders and possibly two or three of\r\nthe brigade commanders, were equally inexperienced in war. The\r\nresult was a Union victory that gave the men who achieved it great\r\nconfidence in themselves ever after.', 'Paragraph 214': 'The enemy fought bravely, but they had started out to defeat and\r\ndestroy an army and capture a position. They failed in both, with\r\nvery heavy loss in killed and wounded, and must have gone back\r\ndiscouraged and convinced that the "Yankee" was not an enemy to be\r\ndespised.', 'Paragraph 215': "After the battle I gave verbal instructions to division\r\ncommanders to let the regiments send out parties to bury their own\r\ndead, and to detail parties, under commissioned officers from each\r\ndivision, to bury the Confederate dead in their respective fronts\r\nand to report the numbers so buried. The latter part of these\r\ninstructions was not carried out by all; but they were by those\r\nsent from Sherman's division, and by some of the parties sent out\r\nby McClernand. The heaviest loss sustained by the enemy was in\r\nfront of these two divisions.", 'Paragraph 216': 'The criticism has often been made that the Union troops should\r\nhave been intrenched at Shiloh. Up to that time the pick and spade\r\nhad been but little resorted to at the West. I had, however, taken\r\nthis subject under consideration soon after re-assuming command in\r\nthe field, and, as already stated, my only military engineer\r\nreported unfavorably. Besides this, the troops with me, officers\r\nand men, needed discipline and drill more than they did experience\r\nwith the pick, shovel and axe. Reinforcements were arriving almost\r\ndaily, composed of troops that had been hastily thrown together\r\ninto companies and regiments—fragments of incomplete\r\norganizations, the men and officers strangers to each other. Under\r\nall these circumstances I concluded that drill and discipline were\r\nworth more to our men than fortifications.', 'Paragraph 217': 'General Buell was a brave, intelligent officer, with as much\r\nprofessional pride and ambition of a commendable sort as I ever\r\nknew. I had been two years at West Point with him, and had served\r\nwith him afterwards, in garrison and in the Mexican war, several\r\nyears more. He was not given in early life or in mature years to\r\nforming intimate acquaintances. He was studious by habit, and\r\ncommanded the confidence and respect of all who knew him. He was a\r\nstrict disciplinarian, and perhaps did not distinguish sufficiently\r\nbetween the volunteer who "enlisted for the war" and the soldier\r\nwho serves in time of peace. One system embraced men who risked\r\nlife for a principle, and often men of social standing, competence,\r\nor wealth and independence of character. The other includes, as a\r\nrule, only men who could not do as well in any other occupation.\r\nGeneral Buell became an object of harsh criticism later, some going\r\nso far as to challenge his loyalty. No one who knew him ever\r\nbelieved him capable of a dishonorable act, and nothing could be\r\nmore dishonorable than to accept high rank and command in war and\r\nthen betray the trust. When I came into command of the army in\r\n1864, I requested the Secretary of War to restore General Buell to\r\nduty.', 'Paragraph 218': 'After the war, during the summer of 1865, I travelled\r\nconsiderably through the North, and was everywhere met by large\r\nnumbers of people. Every one had his opinion about the manner in\r\nwhich the war had been conducted: who among the generals had\r\nfailed, how, and why. Correspondents of the press were ever on hand\r\nto hear every word dropped, and were not always disposed to report\r\ncorrectly what did not confirm their preconceived notions, either\r\nabout the conduct of the war or the individuals concerned in it.\r\nThe opportunity frequently occurred for me to defend General Buell\r\nagainst what I believed to be most unjust charges. On one occasion\r\na correspondent put in my mouth the very charge I had so often\r\nrefuted—of disloyalty. This brought from General Buell a very\r\nsevere retort, which I saw in the New York World some time before I\r\nreceived the letter itself. I could very well understand his\r\ngrievance at seeing untrue and disgraceful charges apparently\r\nsustained by an officer who, at the time, was at the head of the\r\narmy. I replied to him, but not through the press. I kept no copy\r\nof my letter, nor did I ever see it in print; neither did I receive\r\nan answer.', 'Paragraph 219': 'General Albert Sidney Johnston, who commanded the Confederate\r\nforces at the beginning of the battle, was disabled by a wound on\r\nthe afternoon of the first day. This wound, as I understood\r\nafterwards, was not necessarily fatal, or even dangerous. But he\r\nwas a man who would not abandon what he deemed an important trust\r\nin the face of danger and consequently continued in the saddle,\r\ncommanding, until so exhausted by the loss of blood that he had to\r\nbe taken from his horse, and soon after died. The news was not long\r\nin reaching our side and I suppose was quite an encouragement to\r\nthe National soldiers.', 'Paragraph 220': 'I had known Johnston slightly in the Mexican war and later as an\r\nofficer in the regular army. He was a man of high character and\r\nability. His contemporaries at West Point, and officers generally\r\nwho came to know him personally later and who remained on our side,\r\nexpected him to prove the most formidable man to meet that the\r\nConfederacy would produce.', 'Paragraph 221': "I once wrote that nothing occurred in his brief command of an\r\narmy to prove or disprove the high estimate that had been placed\r\nupon his military ability; but after studying the orders and\r\ndispatches of Johnston I am compelled to materially modify my views\r\nof that officer's qualifications as a soldier. My judgment now is\r\nthat he was vacillating and undecided in his actions.", 'Paragraph 222': 'All the disasters in Kentucky and Tennessee were so discouraging\r\nto the authorities in Richmond that Jefferson Davis wrote an\r\nunofficial letter to Johnston expressing his own anxiety and that\r\nof the public, and saying that he had made such defence as was\r\ndictated by long friendship, but that in the absence of a report he\r\nneeded facts. The letter was not a reprimand in direct terms, but\r\nit was evidently as much felt as though it had been one. General\r\nJohnston raised another army as rapidly as he could, and fortified\r\nor strongly intrenched at Corinth. He knew the National troops were\r\npreparing to attack him in his chosen position. But he had\r\nevidently become so disturbed at the results of his operations that\r\nhe resolved to strike out in an offensive campaign which would\r\nrestore all that was lost, and if successful accomplish still more.\r\nWe have the authority of his son and biographer for saying that his\r\nplan was to attack the forces at Shiloh and crush them; then to\r\ncross the Tennessee and destroy the army of Buell, and push the war\r\nacross the Ohio River. The design was a bold one; but we have the\r\nsame authority for saying that in the execution Johnston showed\r\nvacillation and indecision. He left Corinth on the 2d of April and\r\nwas not ready to attack until the 6th. The distance his army had to\r\nmarch was less than twenty miles. Beauregard, his second in\r\ncommand, was opposed to the attack for two reasons: first, he\r\nthought, if let alone the National troops would attack the\r\nConfederates in their intrenchments; second, we were in ground of\r\nour own choosing and would necessarily be intrenched. Johnston not\r\nonly listened to the objection of Beauregard to an attack, but held\r\na council of war on the subject on the morning of the 5th. On the\r\nevening of the same day he was in consultation with some of his\r\ngenerals on the same subject, and still again on the morning of the\r\n6th. During this last consultation, and before a decision had been\r\nreached, the battle began by the National troops opening fire on\r\nthe enemy. This seemed to settle the question as to whether there\r\nwas to be any battle of Shiloh. It also seems to me to settle the\r\nquestion as to whether there was a surprise.', 'Paragraph 223': 'I do not question the personal courage of General Johnston, or\r\nhis ability. But he did not win the distinction predicted for him\r\nby many of his friends. He did prove that as a general he was\r\nover-estimated.', 'Paragraph 224': 'General Beauregard was next in rank to Johnston and succeeded to\r\nthe command, which he retained to the close of the battle and\r\nduring the subsequent retreat on Corinth, as well as in the siege\r\nof that place. His tactics have been severely criticised by\r\nConfederate writers, but I do not believe his fallen chief could\r\nhave done any better under the circumstances. Some of these critics\r\nclaim that Shiloh was won when Johnston fell, and that if he had\r\nnot fallen the army under me would have been annihilated or\r\ncaptured. IFS defeated the Confederates at Shiloh. There is little\r\ndoubt that we would have been disgracefully beaten IF all the\r\nshells and bullets fired by us had passed harmlessly over the enemy\r\nand IF all of theirs had taken effect. Commanding generals are\r\nliable to be killed during engagements; and the fact that when he\r\nwas shot Johnston was leading a brigade to induce it to make a\r\ncharge which had been repeatedly ordered, is evidence that there\r\nwas neither the universal demoralization on our side nor the\r\nunbounded confidence on theirs which has been claimed. There was,\r\nin fact, no hour during the day when I doubted the eventual defeat\r\nof the enemy, although I was disappointed that reinforcements so\r\nnear at hand did not arrive at an earlier hour.', 'Paragraph 225': "The description of the battle of Shiloh given by Colonel Wm.\r\nPreston Johnston is very graphic and well told. The reader will\r\nimagine that he can see each blow struck, a demoralized and broken\r\nmob of Union soldiers, each blow sending the enemy more demoralized\r\nthan ever towards the Tennessee River, which was a little more than\r\ntwo miles away at the beginning of the onset. If the reader does\r\nnot stop to inquire why, with such Confederate success for more\r\nthan twelve hours of hard fighting, the National troops were not\r\nall killed, captured or driven into the river, he will regard the\r\npen picture as perfect. But I witnessed the fight from the National\r\nside from eight o'clock in the morning until night closed the\r\ncontest. I see but little in the description that I can recognize.\r\nThe Confederate troops fought well and deserve commendation enough\r\nfor their bravery and endurance on the 6th of April, without\r\ndetracting from their antagonists or claiming anything more than\r\ntheir just dues.", 'Paragraph 226': "The reports of the enemy show that their condition at the end of\r\nthe first day was deplorable; their losses in killed and wounded\r\nhad been very heavy, and their stragglers had been quite as\r\nnumerous as on the National side, with the difference that those of\r\nthe enemy left the field entirely and were not brought back to\r\ntheir respective commands for many days. On the Union side but few\r\nof the stragglers fell back further than the landing on the river,\r\nand many of these were in line for duty on the second day. The\r\nadmissions of the highest Confederate officers engaged at Shiloh\r\nmake the claim of a victory for them absurd. The victory was not to\r\neither party until the battle was over. It was then a Union\r\nvictory, in which the Armies of the Tennessee and the Ohio both\r\nparticipated. But the Army of the Tennessee fought the entire rebel\r\narmy on the 6th and held it at bay until near night; and night\r\nalone closed the conflict and not the three regiments of Nelson's\r\ndivision.", 'Paragraph 227': 'The Confederates fought with courage at Shiloh, but the\r\nparticular skill claimed I could not and still cannot see; though\r\nthere is nothing to criticise except the claims put forward for it\r\nsince. But the Confederate claimants for superiority in strategy,\r\nsuperiority in generalship and superiority in dash and prowess are\r\nnot so unjust to the Union troops engaged at Shiloh as are many\r\nNorthern writers. The troops on both sides were American, and\r\nunited they need not fear any foreign foe. It is possible that the\r\nSouthern man started in with a little more dash than his Northern\r\nbrother; but he was correspondingly less enduring.', 'Paragraph 228': 'The endeavor of the enemy on the first day was simply to hurl\r\ntheir men against ours—first at one point, then at another,\r\nsometimes at several points at once. This they did with daring and\r\nenergy, until at night the rebel troops were worn out. Our effort\r\nduring the same time was to be prepared to resist assaults wherever\r\nmade. The object of the Confederates on the second day was to get\r\naway with as much of their army and material as possible. Ours then\r\nwas to drive them from our front, and to capture or destroy as\r\ngreat a part as possible of their men and material. We were\r\nsuccessful in driving them back, but not so successful in captures\r\nas if farther pursuit could have been made. As it was, we captured\r\nor recaptured on the second day about as much artillery as we lost\r\non the first; and, leaving out the one great capture of Prentiss,\r\nwe took more prisoners on Monday than the enemy gained from us on\r\nSunday. On the 6th Sherman lost seven pieces of artillery,\r\nMcClernand six, Prentiss eight, and Hurlbut two batteries. On the\r\n7th Sherman captured seven guns, McClernand three and the Army of\r\nthe Ohio twenty.', 'Paragraph 229': "At Shiloh the effective strength of the Union forces on the\r\nmorning of the 6th was 33,000 men. Lew. Wallace brought 5,000 more\r\nafter nightfall. Beauregard reported the enemy's strength at\r\n40,955. According to the custom of enumeration in the South, this\r\nnumber probably excluded every man enlisted as musician or detailed\r\nas guard or nurse, and all commissioned officers—everybody\r\nwho did not carry a musket or serve a cannon. With us everybody in\r\nthe field receiving pay from the government is counted. Excluding\r\nthe troops who fled, panic-stricken, before they had fired a shot,\r\nthere was not a time during the 6th when we had more than 25,000\r\nmen in line. On the 7th Buell brought 20,000 more. Of his remaining\r\ntwo divisions, Thomas's did not reach the field during the\r\nengagement; Wood's arrived before firing had ceased, but not in\r\ntime to be of much service.", 'Paragraph 230': "Our loss in the two days' fight was 1,754 killed, 8,408 wounded\r\nand 2,885 missing. Of these, 2,103 were in the Army of the Ohio.\r\nBeauregard reported a total loss of 10,699, of whom 1,728 were\r\nkilled, 8,012 wounded and 957 missing. This estimate must be\r\nincorrect. We buried, by actual count, more of the enemy's dead in\r\nfront of the divisions of McClernand and Sherman alone than here\r\nreported, and 4,000 was the estimate of the burial parties of the\r\nwhole field. Beauregard reports the Confederate force on the 6th at\r\nover 40,000, and their total loss during the two days at 10,699;\r\nand at the same time declares that he could put only 20,000 men in\r\nbattle on the morning of the 7th.", 'Paragraph 231': 'The navy gave a hearty support to the army at Shiloh, as indeed\r\nit always did both before and subsequently when I was in command.\r\nThe nature of the ground was such, however, that on this occasion\r\nit could do nothing in aid of the troops until sundown on the first\r\nday. The country was broken and heavily timbered, cutting off all\r\nview of the battle from the river, so that friends would be as much\r\nin danger from fire from the gunboats as the foe. But about\r\nsundown, when the National troops were back in their last position,\r\nthe right of the enemy was near the river and exposed to the fire\r\nof the two gun-boats, which was delivered with vigor and effect.\r\nAfter nightfall, when firing had entirely ceased on land, the\r\ncommander of the fleet informed himself, approximately, of the\r\nposition of our troops and suggested the idea of dropping a shell\r\nwithin the lines of the enemy every fifteen minutes during the\r\nnight. This was done with effect, as is proved by the Confederate\r\nreports.', 'Paragraph 232': 'Up to the battle of Shiloh I, as well as thousands of other\r\ncitizens, believed that the rebellion against the Government would\r\ncollapse suddenly and soon, if a decisive victory could be gained\r\nover any of its armies. Donelson and Henry were such victories. An\r\narmy of more than 21,000 men was captured or destroyed. Bowling\r\nGreen, Columbus and Hickman, Kentucky, fell in consequence, and\r\nClarksville and Nashville, Tennessee, the last two with an immense\r\namount of stores, also fell into our hands. The Tennessee and\r\nCumberland rivers, from their mouths to the head of navigation,\r\nwere secured. But when Confederate armies were collected which not\r\nonly attempted to hold a line farther south, from Memphis to\r\nChattanooga, Knoxville and on to the Atlantic, but assumed the\r\noffensive and made such a gallant effort to regain what had been\r\nlost, then, indeed, I gave up all idea of saving the Union except\r\nby complete conquest. Up to that time it had been the policy of our\r\narmy, certainly of that portion commanded by me, to protect the\r\nproperty of the citizens whose territory was invaded, without\r\nregard to their sentiments, whether Union or Secession. After this,\r\nhowever, I regarded it as humane to both sides to protect the\r\npersons of those found at their homes, but to consume everything\r\nthat could be used to support or supply armies. Protection was\r\nstill continued over such supplies as were within lines held by us\r\nand which we expected to continue to hold; but such supplies within\r\nthe reach of Confederate armies I regarded as much contraband as\r\narms or ordnance stores. Their destruction was accomplished without\r\nbloodshed and tended to the same result as the destruction of\r\narmies. I continued this policy to the close of the war.\r\nPromiscuous pillaging, however, was discouraged and punished.\r\nInstructions were always given to take provisions and forage under\r\nthe direction of commissioned officers who should give receipts to\r\nowners, if at home, and turn the property over to officers of the\r\nquartermaster or commissary departments to be issued as if\r\nfurnished from our Northern depots. But much was destroyed without\r\nreceipts to owners, when it could not be brought within our lines\r\nand would otherwise have gone to the support of secession and\r\nrebellion.', 'Paragraph 233': 'This policy I believe exercised a material influence in\r\nhastening the end.', 'Paragraph 234': 'The battle of Shiloh, or Pittsburg landing, has been perhaps\r\nless understood, or, to state the case more accurately, more\r\npersistently misunderstood, than any other engagement between\r\nNational and Confederate troops during the entire rebellion.\r\nCorrect reports of the battle have been published, notably by\r\nSherman, Badeau and, in a speech before a meeting of veterans, by\r\nGeneral Prentiss; but all of these appeared long subsequent to the\r\nclose of the rebellion and after public opinion had been most\r\nerroneously formed.', 'Paragraph 235': 'I myself made no report to General Halleck, further than was\r\ncontained in a letter, written immediately after the battle\r\ninforming him that an engagement had been fought and announcing the\r\nresult. A few days afterwards General Halleck moved his\r\nheadquarters to Pittsburg landing and assumed command of the troops\r\nin the field. Although next to him in rank, and nominally in\r\ncommand of my old district and army, I was ignored as much as if I\r\nhad been at the most distant point of territory within my\r\njurisdiction; and although I was in command of all the troops\r\nengaged at Shiloh I was not permitted to see one of the reports of\r\nGeneral Buell or his subordinates in that battle, until they were\r\npublished by the War Department long after the event. For this\r\nreason I never made a full official report of this engagement.', 'Paragraph 236': "General Halleck arrived at Pittsburg landing on the 11th of\r\nApril and immediately assumed command in the field. On the 21st\r\nGeneral Pope arrived with an army 30,000 strong, fresh from the\r\ncapture of Island Number Ten in the Mississippi River. He went into\r\ncamp at Hamburg landing five miles above Pittsburg. Halleck had now\r\nthree armies: the Army of the Ohio, Buell commanding; the Army of\r\nthe Mississippi, Pope commanding; and the Army of the Tennessee.\r\nHis orders divided the combined force into the right wing, reserve,\r\ncentre and left wing. Major-General George H. Thomas, who had been\r\nin Buell's army, was transferred with his division to the Army of\r\nthe Tennessee and given command of the right wing, composed of all\r\nof that army except McClernand's and Lew. Wallace's divisions.\r\nMcClernand was assigned to the command of the reserve, composed of\r\nhis own and Lew. Wallace's divisions. Buell commanded the centre,\r\nthe Army of the Ohio; and Pope the left wing, the Army of the\r\nMississippi. I was named second in command of the whole, and was\r\nalso supposed to be in command of the right wing and reserve.", 'Paragraph 237': 'Orders were given to all the commanders engaged at Shiloh to\r\nsend in their reports without delay to department headquarters.\r\nThose from officers of the Army of the Tennessee were sent through\r\nme; but from the Army of the Ohio they were sent by General Buell\r\nwithout passing through my hands. General Halleck ordered me,\r\nverbally, to send in my report, but I positively declined on the\r\nground that he had received the reports of a part of the army\r\nengaged at Shiloh without their coming through me. He admitted that\r\nmy refusal was justifiable under the circumstances, but explained\r\nthat he had wanted to get the reports off before moving the\r\ncommand, and as fast as a report had come to him he had forwarded\r\nit to Washington.', 'Paragraph 238': 'Preparations were at once made upon the arrival of the new\r\ncommander for an advance on Corinth. Owl Creek, on our right, was\r\nbridged, and expeditions were sent to the north-west and west to\r\nascertain if our position was being threatened from those quarters;\r\nthe roads towards Corinth were corduroyed and new ones made;\r\nlateral roads were also constructed, so that in case of necessity\r\ntroops marching by different routes could reinforce each other. All\r\ncommanders were cautioned against bringing on an engagement and\r\ninformed in so many words that it would be better to retreat than\r\nto fight. By the 30th of April all preparations were complete; the\r\ncountry west to the Mobile and Ohio railroad had been reconnoitred,\r\nas well as the road to Corinth as far as Monterey twelve miles from\r\nPittsburg. Everywhere small bodies of the enemy had been\r\nencountered, but they were observers and not in force to fight\r\nbattles.', 'Paragraph 239': 'Corinth, Mississippi, lies in a south-westerly direction from\r\nPittsburg landing and about nineteen miles away as the bird would\r\nfly, but probably twenty-two by the nearest wagon-road. It is about\r\nfour miles south of the line dividing the States of Tennessee and\r\nMississippi, and at the junction of the Mississippi and Chattanooga\r\nrailroad with the Mobile and Ohio road which runs from Columbus to\r\nMobile. From Pittsburg to Corinth the land is rolling, but at no\r\npoint reaching an elevation that makes high hills to pass over. In\r\n1862 the greater part of the country was covered with forest with\r\nintervening clearings and houses. Underbrush was dense in the low\r\ngrounds along the creeks and ravines, but generally not so thick on\r\nthe high land as to prevent men passing through with ease. There\r\nare two small creeks running from north of the town and connecting\r\nsome four miles south, where they form Bridge Creek which empties\r\ninto the Tuscumbia River. Corinth is on the ridge between these\r\nstreams and is a naturally strong defensive position. The creeks\r\nare insignificant in volume of water, but the stream to the east\r\nwidens out in front of the town into a swamp impassable in the\r\npresence of an enemy. On the crest of the west bank of this stream\r\nthe enemy was strongly intrenched.', 'Paragraph 240': 'Corinth was a valuable strategic point for the enemy to hold,\r\nand consequently a valuable one for us to possess ourselves of. We\r\nought to have seized it immediately after the fall of Donelson and\r\nNashville, when it could have been taken without a battle, but\r\nfailing then it should have been taken, without delay on the\r\nconcentration of troops at Pittsburg landing after the battle of\r\nShiloh. In fact the arrival of Pope should not have been awaited.\r\nThere was no time from the battle of Shiloh up to the evacuation of\r\nCorinth when the enemy would not have left if pushed. The\r\ndemoralization among the Confederates from their defeats at Henry\r\nand Donelson; their long marches from Bowling Green, Columbus, and\r\nNashville, and their failure at Shiloh; in fact from having been\r\ndriven out of Kentucky and Tennessee, was so great that a stand for\r\nthe time would have been impossible. Beauregard made strenuous\r\nefforts to reinforce himself and partially succeeded. He appealed\r\nto the people of the South-west for new regiments, and received a\r\nfew. A. S. Johnston had made efforts to reinforce in the same\r\nquarter, before the battle of Shiloh, but in a different way. He\r\nhad negroes sent out to him to take the place of teamsters, company\r\ncooks and laborers in every capacity, so as to put all his white\r\nmen into the ranks. The people, while willing to send their sons to\r\nthe field, were not willing to part with their negroes. It is only\r\nfair to state that they probably wanted their blacks to raise\r\nsupplies for the army and for the families left at home.', 'Paragraph 241': 'Beauregard, however, was reinforced by Van Dorn immediately\r\nafter Shiloh with 17,000 men. Interior points, less exposed, were\r\nalso depleted to add to the strength at Corinth. With these\r\nreinforcements and the new regiments, Beauregard had, during the\r\nmonth of May, 1862, a large force on paper, but probably not much\r\nover 50,000 effective men. We estimated his strength at 70,000. Our\r\nown was, in round numbers, 120,000. The defensible nature of the\r\nground at Corinth, and the fortifications, made 50,000 then enough\r\nto maintain their position against double that number for an\r\nindefinite time but for the demoralization spoken of.', 'Paragraph 242': 'On the 30th of April the grand army commenced its advance from\r\nShiloh upon Corinth. The movement was a siege from the start to the\r\nclose. The National troops were always behind intrenchments, except\r\nof course the small reconnoitring parties sent to the front to\r\nclear the way for an advance. Even the commanders of these parties\r\nwere cautioned, "not to bring on an engagement." "It is better to\r\nretreat than to fight." The enemy were constantly watching our\r\nadvance, but as they were simply observers there were but few\r\nengagements that even threatened to become battles. All the\r\nengagements fought ought to have served to encourage the enemy.\r\nRoads were again made in our front, and again corduroyed; a line\r\nwas intrenched, and the troops were advanced to the new position.\r\nCross roads were constructed to these new positions to enable the\r\ntroops to concentrate in case of attack. The National armies were\r\nthoroughly intrenched all the way from the Tennessee River to\r\nCorinth.', 'Paragraph 243': 'For myself I was little more than an observer. Orders were sent\r\ndirect to the right wing or reserve, ignoring me, and advances were\r\nmade from one line of intrenchments to another without notifying\r\nme. My position was so embarrassing in fact that I made several\r\napplications during the siege to be relieved.', 'Paragraph 244': 'General Halleck kept his headquarters generally, if not all the\r\ntime, with the right wing. Pope being on the extreme left did not\r\nsee so much of his chief, and consequently got loose as it were at\r\ntimes. On the 3d of May he was at Seven Mile Creek with the main\r\nbody of his command, but threw forward a division to Farmington,\r\nwithin four miles of Corinth. His troops had quite a little\r\nengagement at Farmington on that day, but carried the place with\r\nconsiderable loss to the enemy. There would then have been no\r\ndifficulty in advancing the centre and right so as to form a new\r\nline well up to the enemy, but Pope was ordered back to conform\r\nwith the general line. On the 8th of May he moved again, taking his\r\nwhole force to Farmington, and pushed out two divisions close to\r\nthe rebel line. Again he was ordered back. By the 4th of May the\r\ncentre and right wing reached Monterey, twelve miles out. Their\r\nadvance was slow from there, for they intrenched with every forward\r\nmovement. The left wing moved up again on the 25th of May and\r\nintrenched itself close to the enemy. The creek with the marsh\r\nbefore described, separated the two lines. Skirmishers thirty feet\r\napart could have maintained either line at this point.', 'Paragraph 245': "Our centre and right were, at this time, extended so that the\r\nright of the right wing was probably five miles from Corinth and\r\nfour from the works in their front. The creek, which was a\r\nformidable obstacle for either side to pass on our left, became a\r\nvery slight obstacle on our right. Here the enemy occupied two\r\npositions. One of them, as much as two miles out from his main\r\nline, was on a commanding elevation and defended by an intrenched\r\nbattery with infantry supports. A heavy wood intervened between\r\nthis work and the National forces. In rear to the south there was a\r\nclearing extending a mile or more, and south of this clearing a\r\nlog-house which had been loop-holed and was occupied by infantry.\r\nSherman's division carried these two positions with some loss to\r\nhimself, but with probably greater to the enemy, on the 28th of\r\nMay, and on that day the investment of Corinth was complete, or as\r\ncomplete as it was ever made. Thomas' right now rested west of the\r\nMobile and Ohio railroad. Pope's left commanded the Memphis and\r\nCharleston railroad east of Corinth.", 'Paragraph 246': 'Some days before I had suggested to the commanding general that\r\nI thought if he would move the Army of the Mississippi at night, by\r\nthe rear of the centre and right, ready to advance at daylight,\r\nPope would find no natural obstacle in his front and, I believed,\r\nno serious artificial one. The ground, or works, occupied by our\r\nleft could be held by a thin picket line, owing to the stream and\r\nswamp in front. To the right the troops would have a dry ridge to\r\nmarch over. I was silenced so quickly that I felt that possibly I\r\nhad suggested an unmilitary movement.', 'Paragraph 247': 'Later, probably on the 28th of May, General Logan, whose command\r\nwas then on the Mobile and Ohio railroad, said to me that the enemy\r\nhad been evacuating for several days and that if allowed he could\r\ngo into Corinth with his brigade. Trains of cars were heard coming\r\nin and going out of Corinth constantly. Some of the men who had\r\nbeen engaged in various capacities on railroads before the war\r\nclaimed that they could tell, by putting their ears to the rail,\r\nnot only which way the trains were moving but which trains were\r\nloaded and which were empty. They said loaded trains had been going\r\nout for several days and empty ones coming in. Subsequent events\r\nproved the correctness of their judgment. Beauregard published his\r\norders for the evacuation of Corinth on the 26th of May and fixed\r\nthe 29th for the departure of his troops, and on the 30th of May\r\nGeneral Halleck had his whole army drawn up prepared for battle and\r\nannounced in orders that there was every indication that our left\r\nwas to be attacked that morning. Corinth had already been evacuated\r\nand the National troops marched on and took possession without\r\nopposition. Everything had been destroyed or carried away. The\r\nConfederate commander had instructed his soldiers to cheer on the\r\narrival of every train to create the impression among the Yankees\r\nthat reinforcements were arriving. There was not a sick or wounded\r\nman left by the Confederates, nor stores of any kind. Some\r\nammunition had been blown up—not removed—but the\r\ntrophies of war were a few Quaker guns, logs of about the diameter\r\nof ordinary cannon, mounted on wheels of wagons and pointed in the\r\nmost threatening manner towards us.', 'Paragraph 248': "The possession of Corinth by the National troops was of\r\nstrategic importance, but the victory was barren in every other\r\nparticular. It was nearly bloodless. It is a question whether the\r\nMORALE of the Confederate troops engaged at Corinth was not\r\nimproved by the immunity with which they were permitted to remove\r\nall public property and then withdraw themselves. On our side I\r\nknow officers and men of the Army of the Tennessee—and I\r\npresume the same is true of those of the other commands—were\r\ndisappointed at the result. They could not see how the mere\r\noccupation of places was to close the war while large and effective\r\nrebel armies existed. They believed that a well-directed attack\r\nwould at least have partially destroyed the army defending Corinth.\r\nFor myself I am satisfied that Corinth could have been captured in\r\na two days' campaign commenced promptly on the arrival of\r\nreinforcements after the battle of Shiloh.", 'Paragraph 249': 'General Halleck at once commenced erecting fortifications around\r\nCorinth on a scale to indicate that this one point must be held if\r\nit took the whole National army to do it. All commanding points two\r\nor three miles to the south, south-east and south-west were\r\nstrongly fortified. It was expected in case of necessity to connect\r\nthese forts by rifle-pits. They were laid out on a scale that would\r\nhave required 100,000 men to fully man them. It was probably\r\nthought that a final battle of the war would be fought at that\r\npoint. These fortifications were never used. Immediately after the\r\noccupation of Corinth by the National troops, General Pope was sent\r\nin pursuit of the retreating garrison and General Buell soon\r\nfollowed. Buell was the senior of the two generals and commanded\r\nthe entire column. The pursuit was kept up for some thirty miles,\r\nbut did not result in the capture of any material of war or\r\nprisoners, unless a few stragglers who had fallen behind and were\r\nwilling captives. On the 10th of June the pursuing column was all\r\nback at Corinth. The Army of the Tennessee was not engaged in any\r\nof these movements.', 'Paragraph 250': 'The Confederates were now driven out of West Tennessee, and on\r\nthe 6th of June, after a well-contested naval battle, the National\r\nforces took possession of Memphis and held the Mississippi river\r\nfrom its source to that point. The railroad from Columbus to\r\nCorinth was at once put in good condition and held by us. We had\r\ngarrisons at Donelson, Clarksville and Nashville, on the Cumberland\r\nRiver, and held the Tennessee River from its mouth to Eastport. New\r\nOrleans and Baton Rouge had fallen into the possession of the\r\nNational forces, so that now the Confederates at the west were\r\nnarrowed down for all communication with Richmond to the single\r\nline of road running east from Vicksburg. To dispossess them of\r\nthis, therefore, became a matter of the first importance. The\r\npossession of the Mississippi by us from Memphis to Baton Rouge was\r\nalso a most important object. It would be equal to the amputation\r\nof a limb in its weakening effects upon the enemy.', 'Paragraph 251': 'After the capture of Corinth a movable force of 80,000 men,\r\nbesides enough to hold all the territory acquired, could have been\r\nset in motion for the accomplishment of any great campaign for the\r\nsuppression of the rebellion. In addition to this fresh troops were\r\nbeing raised to swell the effective force. But the work of\r\ndepletion commenced. Buell with the Army of the Ohio was sent east,\r\nfollowing the line of the Memphis and Charleston railroad. This he\r\nwas ordered to repair as he advanced—only to have it\r\ndestroyed by small guerilla bands or other troops as soon as he was\r\nout of the way. If he had been sent directly to Chattanooga as\r\nrapidly as he could march, leaving two or three divisions along the\r\nline of the railroad from Nashville forward, he could have arrived\r\nwith but little fighting, and would have saved much of the loss of\r\nlife which was afterwards incurred in gaining Chattanooga. Bragg\r\nwould then not have had time to raise an army to contest the\r\npossession of middle and east Tennessee and Kentucky; the battles\r\nof Stone River and Chickamauga would not necessarily have been\r\nfought; Burnside would not have been besieged in Knoxville without\r\nthe power of helping himself or escaping; the battle of Chattanooga\r\nwould not have been fought. These are the negative advantages, if\r\nthe term negative is applicable, which would probably have resulted\r\nfrom prompt movements after Corinth fell into the possession of the\r\nNational forces. The positive results might have been: a bloodless\r\nadvance to Atlanta, to Vicksburg, or to any other desired point\r\nsouth of Corinth in the interior of Mississippi.', 'Paragraph 252': 'My position at Corinth, with a nominal command and yet no\r\ncommand, became so unbearable that I asked permission of Halleck to\r\nremove my headquarters to Memphis. I had repeatedly asked, between\r\nthe fall of Donelson and the evacuation of Corinth, to be relieved\r\nfrom duty under Halleck; but all my applications were refused until\r\nthe occupation of the town. I then obtained permission to leave the\r\ndepartment, but General Sherman happened to call on me as I was\r\nabout starting and urged me so strongly not to think of going, that\r\nI concluded to remain. My application to be permitted to remove my\r\nheadquarters to Memphis was, however, approved, and on the 21st of\r\nJune I started for that point with my staff and a cavalry escort of\r\nonly a part of one company. There was a detachment of two or three\r\ncompanies going some twenty-five miles west to be stationed as a\r\nguard to the railroad. I went under cover of this escort to the end\r\nof their march, and the next morning proceeded to La Grange with no\r\nconvoy but the few cavalry men I had with me.', 'Paragraph 253': 'From La Grange to Memphis the distance is forty-seven miles.\r\nThere were no troops stationed between these two points, except a\r\nsmall force guarding a working party which was engaged in repairing\r\nthe railroad. Not knowing where this party would be found I halted\r\nat La Grange. General Hurlbut was in command there at the time and\r\nhad his headquarters tents pitched on the lawn of a very commodious\r\ncountry house. The proprietor was at home and, learning of my\r\narrival, he invited General Hurlbut and me to dine with him. I\r\naccepted the invitation and spent a very pleasant afternoon with my\r\nhost, who was a thorough Southern gentleman fully convinced of the\r\njustice of secession. After dinner, seated in the capacious porch,\r\nhe entertained me with a recital of the services he was rendering\r\nthe cause. He was too old to be in the ranks himself—he must\r\nhave been quite seventy then—but his means enabled him to be\r\nuseful in other ways. In ordinary times the homestead where he was\r\nnow living produced the bread and meat to supply the slaves on his\r\nmain plantation, in the low-lands of Mississippi. Now he raised\r\nfood and forage on both places, and thought he would have that year\r\na surplus sufficient to feed three hundred families of poor men who\r\nhad gone into the war and left their families dependent upon the\r\n"patriotism" of those better off. The crops around me looked fine,\r\nand I had at the moment an idea that about the time they were ready\r\nto be gathered the "Yankee" troops would be in the neighborhood and\r\nharvest them for the benefit of those engaged in the suppression of\r\nthe rebellion instead of its support. I felt, however, the greatest\r\nrespect for the candor of my host and for his zeal in a cause he\r\nthoroughly believed in, though our views were as wide apart as it\r\nis possible to conceive.', 'Paragraph 254': 'The 23d of June, 1862, on the road from La Grange to Memphis was\r\nvery warm, even for that latitude and season. With my staff and\r\nsmall escort I started at an early hour, and before noon we arrived\r\nwithin twenty miles of Memphis. At this point I saw a very\r\ncomfortable-looking white-haired gentleman seated at the front of\r\nhis house, a little distance from the road. I let my staff and\r\nescort ride ahead while I halted and, for an excuse, asked for a\r\nglass of water. I was invited at once to dismount and come in. I\r\nfound my host very genial and communicative, and staid longer than\r\nI had intended, until the lady of the house announced dinner and\r\nasked me to join them. The host, however, was not pressing, so that\r\nI declined the invitation and, mounting my horse, rode on.', 'Paragraph 255': 'About a mile west from where I had been stopping a road comes up\r\nfrom the southeast, joining that from La Grange to Memphis. A mile\r\nwest of this junction I found my staff and escort halted and\r\nenjoying the shade of forest trees on the lawn of a house located\r\nseveral hundred feet back from the road, their horses hitched to\r\nthe fence along the line of the road. I, too, stopped and we\r\nremained there until the cool of the afternoon, and then rode into\r\nMemphis.', 'Paragraph 256': 'The gentleman with whom I had stopped twenty miles from Memphis\r\nwas a Mr. De Loche, a man loyal to the Union. He had not pressed me\r\nto tarry longer with him because in the early part of my visit a\r\nneighbor, a Dr. Smith, had called and, on being presented to me,\r\nbacked off the porch as if something had hit him. Mr. De Loche knew\r\nthat the rebel General Jackson was in that neighborhood with a\r\ndetachment of cavalry. His neighbor was as earnest in the southern\r\ncause as was Mr. De Loche in that of the Union. The exact location\r\nof Jackson was entirely unknown to Mr. De Loche; but he was sure\r\nthat his neighbor would know it and would give information of my\r\npresence, and this made my stay unpleasant to him after the call of\r\nDr. Smith.', 'Paragraph 257': 'I have stated that a detachment of troops was engaged in\r\nguarding workmen who were repairing the railroad east of Memphis.\r\nOn the day I entered Memphis, Jackson captured a small herd of beef\r\ncattle which had been sent east for the troops so engaged. The\r\ndrovers were not enlisted men and he released them. A day or two\r\nafter one of these drovers came to my headquarters and, relating\r\nthe circumstances of his capture, said Jackson was very much\r\ndisappointed that he had not captured me; that he was six or seven\r\nmiles south of the Memphis and Charleston railroad when he learned\r\nthat I was stopping at the house of Mr. De Loche, and had ridden\r\nwith his command to the junction of the road he was on with that\r\nfrom La Grange and Memphis, where he learned that I had passed\r\nthree-quarters of an hour before. He thought it would be useless to\r\npursue with jaded horses a well-mounted party with so much of a\r\nstart. Had he gone three-quarters of a mile farther he would have\r\nfound me with my party quietly resting under the shade of trees and\r\nwithout even arms in our hands with which to defend ourselves.', 'Paragraph 258': 'General Jackson of course did not communicate his disappointment\r\nat not capturing me to a prisoner, a young drover; but from the\r\ntalk among the soldiers the facts related were learned. A day or\r\ntwo later Mr. De Loche called on me in Memphis to apologize for his\r\napparent incivility in not insisting on my staying for dinner. He\r\nsaid that his wife accused him of marked discourtesy, but that,\r\nafter the call of his neighbor, he had felt restless until I got\r\naway. I never met General Jackson before the war, nor during it,\r\nbut have met him since at his very comfortable summer home at\r\nManitou Springs, Colorado. I reminded him of the above incident,\r\nand this drew from him the response that he was thankful now he had\r\nnot captured me. I certainly was very thankful too.', 'Paragraph 259': 'My occupation of Memphis as district headquarters did not last\r\nlong. The period, however, was marked by a few incidents which were\r\nnovel to me. Up to that time I had not occupied any place in the\r\nSouth where the citizens were at home in any great numbers. Dover\r\nwas within the fortifications at Fort Donelson, and, as far as I\r\nremember, every citizen was gone. There were no people living at\r\nPittsburg landing, and but very few at Corinth. Memphis, however,\r\nwas a populous city, and there were many of the citizens remaining\r\nthere who were not only thoroughly impressed with the justice of\r\ntheir cause, but who thought that even the "Yankee soldiery" must\r\nentertain the same views if they could only be induced to make an\r\nhonest confession. It took hours of my time every day to listen to\r\ncomplaints and requests. The latter were generally reasonable, and\r\nif so they were granted; but the complaints were not always, or\r\neven often, well founded. Two instances will mark the general\r\ncharacter. First: the officer who commanded at Memphis immediately\r\nafter the city fell into the hands of the National troops had\r\nordered one of the churches of the city to be opened to the\r\nsoldiers. Army chaplains were authorized to occupy the pulpit.\r\nSecond: at the beginning of the war the Confederate Congress had\r\npassed a law confiscating all property of "alien enemies" at the\r\nSouth, including the debts of Southerners to Northern men. In\r\nconsequence of this law, when Memphis was occupied the\r\nprovost-marshal had forcibly collected all the evidences he could\r\nobtain of such debts.', 'Paragraph 260': 'Almost the first complaints made to me were these two outrages.\r\nThe gentleman who made the complaints informed me first of his own\r\nhigh standing as a lawyer, a citizen and a Christian. He was a\r\ndeacon in the church which had been defiled by the occupation of\r\nUnion troops, and by a Union chaplain filling the pulpit. He did\r\nnot use the word "defile," but he expressed the idea very clearly.\r\nHe asked that the church be restored to the former congregation. I\r\ntold him that no order had been issued prohibiting the congregation\r\nattending the church. He said of course the congregation could not\r\nhear a Northern clergyman who differed so radically with them on\r\nquestions of government. I told him the troops would continue to\r\noccupy that church for the present, and that they would not be\r\ncalled upon to hear disloyal sentiments proclaimed from the pulpit.\r\nThis closed the argument on the first point.', 'Paragraph 261': 'Then came the second. The complainant said that he wanted the\r\npapers restored to him which had been surrendered to the\r\nprovost-marshal under protest; he was a lawyer, and before the\r\nestablishment of the "Confederate States Government" had been the\r\nattorney for a number of large business houses at the North; that\r\n"his government" had confiscated all debts due "alien enemies," and\r\nappointed commissioners, or officers, to collect such debts and pay\r\nthem over to the "government": but in his case, owing to his high\r\nstanding, he had been permitted to hold these claims for\r\ncollection, the responsible officials knowing that he would account\r\nto the "government" for every dollar received. He said that his\r\n"government," when it came in possession of all its territory,\r\nwould hold him personally responsible for the claims he had\r\nsurrendered to the provost-marshal. His impudence was so sublime\r\nthat I was rather amused than indignant. I told him, however, that\r\nif he would remain in Memphis I did not believe the Confederate\r\ngovernment would ever molest him. He left, no doubt, as much amazed\r\nat my assurance as I was at the brazenness of his request.', 'Paragraph 262': 'On the 11th of July General Halleck received telegraphic orders\r\nappointing him to the command of all the armies, with headquarters\r\nin Washington. His instructions pressed him to proceed to his new\r\nfield of duty with as little delay as was consistent with the\r\nsafety and interests of his previous command. I was next in rank,\r\nand he telegraphed me the same day to report at department\r\nheadquarters at Corinth. I was not informed by the dispatch that my\r\nchief had been ordered to a different field and did not know\r\nwhether to move my headquarters or not. I telegraphed asking if I\r\nwas to take my staff with me, and received word in reply: "This\r\nplace will be your headquarters. You can judge for yourself." I\r\nleft Memphis for my new field without delay, and reached Corinth on\r\nthe 15th of the month. General Halleck remained until the 17th of\r\nJuly; but he was very uncommunicative, and gave me no information\r\nas to what I had been called to Corinth for.', 'Paragraph 263': 'When General Halleck left to assume the duties of\r\ngeneral-in-chief I remained in command of the district of West\r\nTennessee. Practically I became a department commander, because no\r\none was assigned to that position over me and I made my reports\r\ndirect to the general-in-chief; but I was not assigned to the\r\nposition of department commander until the 25th of October. General\r\nHalleck while commanding the Department of the Mississippi had had\r\ncontrol as far east as a line drawn from Chattanooga north. My\r\ndistrict only embraced West Tennessee and Kentucky west of the\r\nCumberland River. Buell, with the Army of the Ohio, had, as\r\npreviously stated, been ordered east towards Chattanooga, with\r\ninstructions to repair the Memphis and Charleston railroad as he\r\nadvanced. Troops had been sent north by Halleck along the line of\r\nthe Mobile and Ohio railroad to put it in repair as far as\r\nColumbus. Other troops were stationed on the railroad from Jackson,\r\nTennessee, to Grand Junction, and still others on the road west to\r\nMemphis.', 'Paragraph 264': 'The remainder of the magnificent army of 120,000 men which\r\nentered Corinth on the 30th of May had now become so scattered that\r\nI was put entirely on the defensive in a territory whose population\r\nwas hostile to the Union. One of the first things I had to do was\r\nto construct fortifications at Corinth better suited to the\r\ngarrison that could be spared to man them. The structures that had\r\nbeen built during the months of May and June were left as monuments\r\nto the skill of the engineer, and others were constructed in a few\r\ndays, plainer in design but suited to the command available to\r\ndefend them.', 'Paragraph 265': 'I disposed the troops belonging to the district in conformity\r\nwith the situation as rapidly as possible. The forces at Donelson,\r\nClarksville and Nashville, with those at Corinth and along the\r\nrailroad eastward, I regarded as sufficient for protection against\r\nany attack from the west. The Mobile and Ohio railroad was guarded\r\nfrom Rienzi, south of Corinth, to Columbus; and the Mississippi\r\nCentral railroad from Jackson, Tennessee, to Bolivar. Grand\r\nJunction and La Grange on the Memphis railroad were abandoned.', 'Paragraph 266': "South of the Army of the Tennessee, and confronting it, was Van\r\nDorn, with a sufficient force to organize a movable army of\r\nthirty-five to forty thousand men, after being reinforced by Price\r\nfrom Missouri. This movable force could be thrown against either\r\nCorinth, Bolivar or Memphis; and the best that could be done in\r\nsuch event would be to weaken the points not threatened in order to\r\nreinforce the one that was. Nothing could be gained on the National\r\nside by attacking elsewhere, because the territory already occupied\r\nwas as much as the force present could guard. The most anxious\r\nperiod of the war, to me, was during the time the Army of the\r\nTennessee was guarding the territory acquired by the fall of\r\nCorinth and Memphis and before I was sufficiently reinforced to\r\ntake the offensive. The enemy also had cavalry operating in our\r\nrear, making it necessary to guard every point of the railroad back\r\nto Columbus, on the security of which we were dependent for all our\r\nsupplies. Headquarters were connected by telegraph with all points\r\nof the command except Memphis and the Mississippi below Columbus.\r\nWith these points communication was had by the railroad to\r\nColumbus, then down the river by boat. To reinforce Memphis would\r\ntake three or four days, and to get an order there for troops to\r\nmove elsewhere would have taken at least two days. Memphis\r\ntherefore was practically isolated from the balance of the command.\r\nBut it was in Sherman's hands. Then too the troops were well\r\nintrenched and the gunboats made a valuable auxiliary.", 'Paragraph 267': 'During the two months after the departure of General Halleck\r\nthere was much fighting between small bodies of the contending\r\narmies, but these encounters were dwarfed by the magnitude of the\r\nmain battles so as to be now almost forgotten except by those\r\nengaged in them. Some of them, however, estimated by the losses on\r\nboth sides in killed and wounded, were equal in hard fighting to\r\nmost of the battles of the Mexican war which attracted so much of\r\nthe attention of the public when they occurred. About the 23d of\r\nJuly Colonel Ross, commanding at Bolivar, was threatened by a large\r\nforce of the enemy so that he had to be reinforced from Jackson and\r\nCorinth. On the 27th there was skirmishing on the Hatchie River,\r\neight miles from Bolivar. On the 30th I learned from Colonel P. H.\r\nSheridan, who had been far to the south, that Bragg in person was\r\nat Rome, Georgia, with his troops moving by rail (by way of Mobile)\r\nto Chattanooga and his wagon train marching overland to join him at\r\nRome. Price was at this time at Holly Springs, Mississippi, with a\r\nlarge force, and occupied Grand Junction as an outpost. I proposed\r\nto the general-in-chief to be permitted to drive him away, but was\r\ninformed that, while I had to judge for myself, the best use to\r\nmake of my troops WAS NOT TO SCATTER THEM, but hold them ready to\r\nreinforce Buell.', 'Paragraph 268': "The movement of Bragg himself with his wagon trains to\r\nChattanooga across country, while his troops were transported over\r\na long round-about road to the same destination, without need of\r\nguards except when in my immediate front, demonstrates the\r\nadvantage which troops enjoy while acting in a country where the\r\npeople are friendly. Buell was marching through a hostile region\r\nand had to have his communications thoroughly guarded back to a\r\nbase of supplies. More men were required the farther the National\r\ntroops penetrated into the enemy's country. I, with an army\r\nsufficiently powerful to have destroyed Bragg, was purely on the\r\ndefensive and accomplishing no more than to hold a force far\r\ninferior to my own.", 'Paragraph 269': 'On the 2d of August I was ordered from Washington to live upon\r\nthe country, on the resources of citizens hostile to the\r\ngovernment, so far as practicable. I was also directed to "handle\r\nrebels within our lines without gloves," to imprison them, or to\r\nexpel them from their homes and from our lines. I do not recollect\r\nhaving arrested and confined a citizen (not a soldier) during the\r\nentire rebellion. I am aware that a great many were sent to\r\nnorthern prisons, particularly to Joliet, Illinois, by some of my\r\nsubordinates with the statement that it was my order. I had all\r\nsuch released the moment I learned of their arrest; and finally\r\nsent a staff officer north to release every prisoner who was said\r\nto be confined by my order. There were many citizens at home who\r\ndeserved punishment because they were soldiers when an opportunity\r\nwas afforded to inflict an injury to the National cause. This class\r\nwas not of the kind that were apt to get arrested, and I deemed it\r\nbetter that a few guilty men should escape than that a great many\r\ninnocent ones should suffer.', 'Paragraph 270': 'On the 14th of August I was ordered to send two more divisions\r\nto Buell. They were sent the same day by way of Decatur. On the 22d\r\nColonel Rodney Mason surrendered Clarksville with six companies of\r\nhis regiment.', 'Paragraph 271': 'Colonel Mason was one of the officers who had led their\r\nregiments off the field at almost the first fire of the rebels at\r\nShiloh. He was by nature and education a gentleman, and was\r\nterribly mortified at his action when the battle was over. He came\r\nto me with tears in his eyes and begged to be allowed to have\r\nanother trial. I felt great sympathy for him and sent him, with his\r\nregiment, to garrison Clarksville and Donelson. He selected\r\nClarksville for his headquarters, no doubt because he regarded it\r\nas the post of danger, it being nearer the enemy. But when he was\r\nsummoned to surrender by a band of guerillas, his constitutional\r\nweakness overcame him. He inquired the number of men the enemy had,\r\nand receiving a response indicating a force greater than his own he\r\nsaid if he could be satisfied of that fact he would surrender.\r\nArrangements were made for him to count the guerillas, and having\r\nsatisfied himself that the enemy had the greater force he\r\nsurrendered and informed his subordinate at Donelson of the fact,\r\nadvising him to do the same. The guerillas paroled their prisoners\r\nand moved upon Donelson, but the officer in command at that point\r\nmarched out to meet them and drove them away.', 'Paragraph 272': 'Among other embarrassments, at the time of which I now write,\r\nwas the fact that the government wanted to get out all the cotton\r\npossible from the South and directed me to give every facility\r\ntoward that end. Pay in gold was authorized, and stations on the\r\nMississippi River and on the railroad in our possession had to be\r\ndesignated where cotton would be received. This opened to the enemy\r\nnot only the means of converting cotton into money, which had a\r\nvalue all over the world and which they so much needed, but it\r\nafforded them means of obtaining accurate and intelligent\r\ninformation in regard to our position and strength. It was also\r\ndemoralizing to the troops. Citizens obtaining permits from the\r\ntreasury department had to be protected within our lines and given\r\nfacilities to get out cotton by which they realized enormous\r\nprofits. Men who had enlisted to fight the battles of their country\r\ndid not like to be engaged in protecting a traffic which went to\r\nthe support of an enemy they had to fight, and the profits of which\r\nwent to men who shared none of their dangers.', 'Paragraph 273': "On the 30th of August Colonel M. D. Leggett, near Bolivar, with\r\nthe 20th and 29th Ohio volunteer infantry, was attacked by a force\r\nsupposed to be about 4,000 strong. The enemy was driven away with a\r\nloss of more than one hundred men. On the 1st of September the\r\nbridge guard at Medon was attacked by guerillas. The guard held the\r\nposition until reinforced, when the enemy were routed leaving about\r\nfifty of their number on the field dead or wounded, our loss being\r\nonly two killed and fifteen wounded. On the same day Colonel\r\nDennis, with a force of less than 500 infantry and two pieces of\r\nartillery, met the cavalry of the enemy in strong force, a few\r\nmiles west of Medon, and drove them away with great loss. Our\r\ntroops buried 179 of the enemy's dead, left upon the field.\r\nAfterwards it was found that all the houses in the vicinity of the\r\nbattlefield were turned into hospitals for the wounded. Our loss,\r\nas reported at the time, was forty-five killed and wounded. On the\r\n2d of September I was ordered to send more reinforcements to Buell.\r\nJackson and Bolivar were yet threatened, but I sent the\r\nreinforcements. On the 4th I received direct orders to send\r\nGranger's division also to Louisville, Kentucky.", 'Paragraph 274': "General Buell had left Corinth about the 10th of June to march\r\nupon Chattanooga; Bragg, who had superseded Beauregard in command,\r\nsent one division from Tupelo on the 27th of June for the same\r\nplace. This gave Buell about seventeen days' start. If he had not\r\nbeen required to repair the railroad as he advanced, the march\r\ncould have been made in eighteen days at the outside, and\r\nChattanooga must have been reached by the National forces before\r\nthe rebels could have possibly got there. The road between\r\nNashville and Chattanooga could easily have been put in repair by\r\nother troops, so that communication with the North would have been\r\nopened in a short time after the occupation of the place by the\r\nNational troops. If Buell had been permitted to move in the first\r\ninstance, with the whole of the Army of the Ohio and that portion\r\nof the Army of the Mississippi afterwards sent to him, he could\r\nhave thrown four divisions from his own command along the line of\r\nroad to repair and guard it.", 'Paragraph 275': "Granger's division was promptly sent on the 4th of September. I\r\nwas at the station at Corinth when the troops reached that point,\r\nand found General P. H. Sheridan with them. I expressed surprise at\r\nseeing him and said that I had not expected him to go. He showed\r\ndecided disappointment at the prospect of being detained. I felt a\r\nlittle nettled at his desire to get away and did not detain\r\nhim.", 'Paragraph 276': 'Sheridan was a first lieutenant in the regiment in which I had\r\nserved eleven years, the 4th infantry, and stationed on the Pacific\r\ncoast when the war broke out. He was promoted to a captaincy in\r\nMay, 1861, and before the close of the year managed in some way, I\r\ndo not know how, to get East. He went to Missouri. Halleck had\r\nknown him as a very successful young officer in managing campaigns\r\nagainst the Indians on the Pacific coast, and appointed him\r\nacting-quartermaster in south-west Missouri. There was no\r\ndifficulty in getting supplies forward while Sheridan served in\r\nthat capacity; but he got into difficulty with his immediate\r\nsuperiors because of his stringent rules for preventing the use of\r\npublic transportation for private purposes. He asked to be relieved\r\nfrom further duty in the capacity in which he was engaged and his\r\nrequest was granted. When General Halleck took the field in April,\r\n1862, Sheridan was assigned to duty on his staff. During the\r\nadvance on Corinth a vacancy occurred in the colonelcy of the 2d\r\nMichigan cavalry. Governor Blair, of Michigan, telegraphed General\r\nHalleck asking him to suggest the name of a professional soldier\r\nfor the vacancy, saying he would appoint a good man without\r\nreference to his State. Sheridan was named; and was so\r\nconspicuously efficient that when Corinth was reached he was\r\nassigned to command a cavalry brigade in the Army of the\r\nMississippi. He was in command at Booneville on the 1st of July\r\nwith two small regiments, when he was attacked by a force full\r\nthree times as numerous as his own. By very skilful manoeuvres and\r\nboldness of attack he completely routed the enemy. For this he was\r\nmade a brigadier-general and became a conspicuous figure in the\r\narmy about Corinth. On this account I was sorry to see him leaving\r\nme. His departure was probably fortunate, for he rendered\r\ndistinguished services in his new field.', 'Paragraph 277': 'Granger and Sheridan reached Louisville before Buell got there,\r\nand on the night of their arrival Sheridan with his command threw\r\nup works around the railroad station for the defence of troops as\r\nthey came from the front.', 'Paragraph 278': "At this time, September 4th, I had two divisions of the Army of\r\nthe Mississippi stationed at Corinth, Rienzi, Jacinto and Danville.\r\nThere were at Corinth also Davies' division and two brigades of\r\nMcArthur's, besides cavalry and artillery. This force constituted\r\nmy left wing, of which Rosecrans was in command. General Ord\r\ncommanded the centre, from Bethel to Humboldt on the Mobile and\r\nOhio railroad and from Jackson to Bolivar where the Mississippi\r\nCentral is crossed by the Hatchie River. General Sherman commanded\r\non the right at Memphis with two of his brigades back at\r\nBrownsville, at the crossing of the Hatchie River by the Memphis\r\nand Ohio railroad. This made the most convenient arrangement I\r\ncould devise for concentrating all my spare forces upon any\r\nthreatened point. All the troops of the command were within\r\ntelegraphic communication of each other, except those under\r\nSherman. By bringing a portion of his command to Brownsville, from\r\nwhich point there was a railroad and telegraph back to Memphis,\r\ncommunication could be had with that part of my command within a\r\nfew hours by the use of couriers. In case it became necessary to\r\nreinforce Corinth, by this arrangement all the troops at Bolivar,\r\nexcept a small guard, could be sent by rail by the way of Jackson\r\nin less than twenty-four hours; while the troops from Brownsville\r\ncould march up to Bolivar to take their place.", 'Paragraph 279': 'On the 7th of September I learned of the advance of Van Dorn and\r\nPrice, apparently upon Corinth. One division was brought from\r\nMemphis to Bolivar to meet any emergency that might arise from this\r\nmove of the enemy. I was much concerned because my first duty,\r\nafter holding the territory acquired within my command, was to\r\nprevent further reinforcing of Bragg in Middle Tennessee. Already\r\nthe Army of Northern Virginia had defeated the army under General\r\nPope and was invading Maryland. In the Centre General Buell was on\r\nhis way to Louisville and Bragg marching parallel to him with a\r\nlarge Confederate force for the Ohio River.', 'Paragraph 280': 'I had been constantly called upon to reinforce Buell until at\r\nthis time my entire force numbered less than 50,000 men, of all\r\narms. This included everything from Cairo south within my\r\njurisdiction. If I too should be driven back, the Ohio River would\r\nbecome the line dividing the belligerents west of the Alleghanies,\r\nwhile at the East the line was already farther north than when\r\nhostilities commenced at the opening of the war. It is true\r\nNashville was never given up after its first capture, but it would\r\nhave been isolated and the garrison there would have been obliged\r\nto beat a hasty retreat if the troops in West Tennessee had been\r\ncompelled to fall back. To say at the end of the second year of the\r\nwar the line dividing the contestants at the East was pushed north\r\nof Maryland, a State that had not seceded, and at the West beyond\r\nKentucky, another State which had been always loyal, would have\r\nbeen discouraging indeed. As it was, many loyal people despaired in\r\nthe fall of 1862 of ever saving the Union. The administration at\r\nWashington was much concerned for the safety of the cause it held\r\nso dear. But I believe there was never a day when the President did\r\nnot think that, in some way or other, a cause so just as ours would\r\ncome out triumphant.', 'Paragraph 281': 'Up to the 11th of September Rosecrans still had troops on the\r\nrailroad east of Corinth, but they had all been ordered in. By the\r\n12th all were in except a small force under Colonel Murphy of the\r\n8th Wisconsin. He had been detained to guard the remainder of the\r\nstores which had not yet been brought in to Corinth.', 'Paragraph 282': "On the 13th of September General Sterling Price entered Iuka, a\r\ntown about twenty miles east of Corinth on the Memphis and\r\nCharleston railroad. Colonel Murphy with a few men was guarding the\r\nplace. He made no resistance, but evacuated the town on the\r\napproach of the enemy. I was apprehensive lest the object of the\r\nrebels might be to get troops into Tennessee to reinforce Bragg, as\r\nit was afterwards ascertained to be. The authorities at Washington,\r\nincluding the general-in-chief of the army, were very anxious, as I\r\nhave said, about affairs both in East and Middle Tennessee; and my\r\nanxiety was quite as great on their account as for any danger\r\nthreatening my command. I had not force enough at Corinth to attack\r\nPrice even by stripping everything; and there was danger that\r\nbefore troops could be got from other points he might be far on his\r\nway across the Tennessee. To prevent this all spare forces at\r\nBolivar and Jackson were ordered to Corinth, and cars were\r\nconcentrated at Jackson for their transportation. Within\r\ntwenty-four hours from the transmission of the order the troops\r\nwere at their destination, although there had been a delay of four\r\nhours resulting from the forward train getting off the track and\r\nstopping all the others. This gave a reinforcement of near 8,000\r\nmen, General Ord in command. General Rosecrans commanded the\r\ndistrict of Corinth with a movable force of about 9,000 independent\r\nof the garrison deemed necessary to be left behind. It was known\r\nthat General Van Dorn was about a four days' march south of us,\r\nwith a large force. It might have been part of his plan to attack\r\nat Corinth, Price coming from the east while he came up from the\r\nsouth. My desire was to attack Price before Van Dorn could reach\r\nCorinth or go to his relief.", 'Paragraph 283': "General Rosecrans had previously had his headquarters at Iuka,\r\nwhere his command was spread out along the Memphis and Charleston\r\nrailroad eastward. While there he had a most excellent map prepared\r\nshowing all the roads and streams in the surrounding country. He\r\nwas also personally familiar with the ground, so that I deferred\r\nvery much to him in my plans for the approach. We had cars enough\r\nto transport all of General Ord's command, which was to go by rail\r\nto Burnsville, a point on the road about seven miles west of Iuka.\r\nFrom there his troops were to march by the north side of the\r\nrailroad and attack Price from the north-west, while Rosecrans was\r\nto move eastward from his position south of Corinth by way of the\r\nJacinto road. A small force was to hold the Jacinto road where it\r\nturns to the north-east, while the main force moved on the Fulton\r\nroad which comes into Iuka further east. This plan was suggested by\r\nRosecrans.", 'Paragraph 284': "Bear Creek, a few miles to the east of the Fulton road, is a\r\nformidable obstacle to the movement of troops in the absence of\r\nbridges, all of which, in September, 1862, had been destroyed in\r\nthat vicinity. The Tennessee, to the north-east, not many miles\r\naway, was also a formidable obstacle for an army followed by a\r\npursuing force. Ord was on the north-west, and even if a rebel\r\nmovement had been possible in that direction it could have brought\r\nonly temporary relief, for it would have carried Price's army to\r\nthe rear of the National forces and isolated it from all support.\r\nIt looked to me that, if Price would remain in Iuka until we could\r\nget there, his annihilation was inevitable.", 'Paragraph 285': "On the morning of the 18th of September General Ord moved by\r\nrail to Burnsville, and there left the cars and moved out to\r\nperform his part of the programme. He was to get as near the enemy\r\nas possible during the day and intrench himself so as to hold his\r\nposition until the next morning. Rosecrans was to be up by the\r\nmorning of the 19th on the two roads before described, and the\r\nattack was to be from all three quarters simultaneously. Troops\r\nenough were left at Jacinto and Rienzi to detain any cavalry that\r\nVan Dorn might send out to make a sudden dash into Corinth until I\r\ncould be notified. There was a telegraph wire along the railroad,\r\nso there would be no delay in communication. I detained cars and\r\nlocomotives enough at Burnsville to transport the whole of Ord's\r\ncommand at once, and if Van Dorn had moved against Corinth instead\r\nof Iuka I could have thrown in reinforcements to the number of\r\n7,000 or 8,000 before he could have arrived. I remained at\r\nBurnsville with a detachment of about 900 men from Ord's command\r\nand communicated with my two wings by courier. Ord met the advance\r\nof the enemy soon after leaving Burnsville. Quite a sharp\r\nengagement ensued, but he drove the rebels back with considerable\r\nloss, including one general officer killed. He maintained his\r\nposition and was ready to attack by daylight the next morning. I\r\nwas very much disappointed at receiving a dispatch from Rosecrans\r\nafter midnight from Jacinto, twenty-two miles from Iuka, saying\r\nthat some of his command had been delayed, and that the rear of his\r\ncolumn was not yet up as far as Jacinto. He said, however, that he\r\nwould still be at Iuka by two o'clock the next day. I did not\r\nbelieve this possible because of the distance and the condition of\r\nthe roads, which was bad; besides, troops after a forced march of\r\ntwenty miles are not in a good condition for fighting the moment\r\nthey get through. It might do in marching to relieve a beleaguered\r\ngarrison, but not to make an assault. I immediately sent Ord a copy\r\nof Rosecrans' dispatch and ordered him to be in readiness to attack\r\nthe moment he heard the sound of guns to the south or south-east.\r\nHe was instructed to notify his officers to be on the alert for any\r\nindications of battle. During the 19th the wind blew in the wrong\r\ndirection to transmit sound either towards the point where Ord was,\r\nor to Burnsville where I had remained.", 'Paragraph 286': "A couple of hours before dark on the 19th Rosecrans arrived with\r\nthe head of his column at Barnets, the point where the Jacinto road\r\nto Iuka leaves the road going east. He here turned north without\r\nsending any troops to the Fulton road. While still moving in column\r\nup the Jacinto road he met a force of the enemy and had his advance\r\nbadly beaten and driven back upon the main road. In this short\r\nengagement his loss was considerable for the number engaged, and\r\none battery was taken from him. The wind was still blowing hard and\r\nin the wrong direction to transmit sounds towards either Ord or me.\r\nNeither he nor I nor any one in either command heard a gun that was\r\nfired upon the battle-field. After the engagement Rosecrans sent me\r\na dispatch announcing the result. This was brought by a courier.\r\nThere was no road between Burnsville and the position then occupied\r\nby Rosecrans and the country was impassable for a man on horseback.\r\nThe courier bearing the message was compelled to move west nearly\r\nto Jacinto before he found a road leading to Burnsville. This made\r\nit a late hour of the night before I learned of the battle that had\r\ntaken place during the afternoon. I at once notified Ord of the\r\nfact and ordered him to attack early in the morning. The next\r\nmorning Rosecrans himself renewed the attack and went into Iuka\r\nwith but little resistance. Ord also went in according to orders,\r\nwithout hearing a gun from the south of town but supposing the\r\ntroops coming from the south-west must be up by that time.\r\nRosecrans, however, had put no troops upon the Fulton road, and the\r\nenemy had taken advantage of this neglect and retreated by that\r\nroad during the night. Word was soon brought to me that our troops\r\nwere in Iuka. I immediately rode into town and found that the enemy\r\nwas not being pursued even by the cavalry. I ordered pursuit by the\r\nwhole of Rosecrans' command and went on with him a few miles in\r\nperson. He followed only a few miles after I left him and then went\r\ninto camp, and the pursuit was continued no further. I was\r\ndisappointed at the result of the battle of Iuka—but I had so\r\nhigh an opinion of General Rosecrans that I found no fault at the\r\ntime.", 'Paragraph 287': 'On the 19th of September General Geo. H. Thomas was ordered east\r\nto reinforce Buell. This threw the army at my command still more on\r\nthe defensive. The Memphis and Charleston railroad was abandoned,\r\nexcept at Corinth, and small forces were left at Chewalla and Grand\r\nJunction. Soon afterwards the latter of these two places was given\r\nup and Bolivar became our most advanced position on the Mississippi\r\nCentral railroad. Our cavalry was kept well to the front and\r\nfrequent expeditions were sent out to watch the movements of the\r\nenemy. We were in a country where nearly all the people, except the\r\nnegroes, were hostile to us and friendly to the cause we were\r\ntrying to suppress. It was easy, therefore, for the enemy to get\r\nearly information of our every move. We, on the contrary, had to go\r\nafter our information in force, and then often returned without\r\nit.', 'Paragraph 288': "On the 22d Bolivar was threatened by a large force from south of\r\nGrand Junction, supposed to be twenty regiments of infantry with\r\ncavalry and artillery. I reinforced Bolivar, and went to Jackson in\r\nperson to superintend the movement of troops to whatever point the\r\nattack might be made upon. The troops from Corinth were brought up\r\nin time to repel the threatened movement without a battle. Our\r\ncavalry followed the enemy south of Davis' mills in\r\nMississippi.", 'Paragraph 289': "On the 30th I found that Van Dorn was apparently endeavoring to\r\nstrike the Mississippi River above Memphis. At the same time other\r\npoints within my command were so threatened that it was impossible\r\nto concentrate a force to drive him away. There was at this\r\njuncture a large Union force at Helena, Arkansas, which, had it\r\nbeen within my command, I could have ordered across the river to\r\nattack and break up the Mississippi Central railroad far to the\r\nsouth. This would not only have called Van Dorn back, but would\r\nhave compelled the retention of a large rebel force far to the\r\nsouth to prevent a repetition of such raids on the enemy's line of\r\nsupplies. Geographical lines between the commands during the\r\nrebellion were not always well chosen, or they were too rigidly\r\nadhered to.", 'Paragraph 290': 'Van Dorn did not attempt to get upon the line above Memphis, as\r\nhad apparently been his intention. He was simply covering a deeper\r\ndesign; one much more important to his cause. By the 1st of October\r\nit was fully apparent that Corinth was to be attacked with great\r\nforce and determination, and that Van Dorn, Lovell, Price,\r\nVillepigue and Rust had joined their strength for this purpose.\r\nThere was some skirmishing outside of Corinth with the advance of\r\nthe enemy on the 3d. The rebels massed in the north-west angle of\r\nthe Memphis and Charleston and the Mobile and Ohio railroads, and\r\nwere thus between the troops at Corinth and all possible\r\nreinforcements. Any fresh troops for us must come by a circuitous\r\nroute.', 'Paragraph 291': "On the night of the 3d, accordingly, I ordered General\r\nMcPherson, who was at Jackson, to join Rosecrans at Corinth with\r\nreinforcements picked up along the line of the railroad equal to a\r\nbrigade. Hurlbut had been ordered from Bolivar to march for the\r\nsame destination; and as Van Dorn was coming upon Corinth from the\r\nnorth-west some of his men fell in with the advance of Hurlbut's\r\nand some skirmishing ensued on the evening of the 3d. On the 4th\r\nVan Dorn made a dashing attack, hoping, no doubt, to capture\r\nRosecrans before his reinforcements could come up. In that case the\r\nenemy himself could have occupied the defences of Corinth and held\r\nat bay all the Union troops that arrived. In fact he could have\r\ntaken the offensive against the reinforcements with three or four\r\ntimes their number and still left a sufficient garrison in the\r\nworks about Corinth to hold them. He came near success, some of his\r\ntroops penetrating the National lines at least once, but the works\r\nthat were built after Halleck's departure enabled Rosecrans to hold\r\nhis position until the troops of both McPherson and Hurlbut\r\napproached towards the rebel front and rear. The enemy was finally\r\ndriven back with great slaughter: all their charges, made with\r\ngreat gallantry, were repulsed. The loss on our side was heavy, but\r\nnothing to compare with Van Dorn's. McPherson came up with the\r\ntrain of cars bearing his command as close to the enemy as was\r\nprudent, debarked on the rebel flank and got in to the support of\r\nRosecrans just after the repulse. His approach, as well as that of\r\nHurlbut, was known to the enemy and had a moral effect. General\r\nRosecrans, however, failed to follow up the victory, although I had\r\ngiven specific orders in advance of the battle for him to pursue\r\nthe moment the enemy was repelled. He did not do so, and I repeated\r\nthe order after the battle. In the first order he was notified that\r\nthe force of 4,000 men which was going to his assistance would be\r\nin great peril if the enemy was not pursued.", 'Paragraph 292': "General Ord had joined Hurlbut on the 4th and being senior took\r\ncommand of his troops. This force encountered the head of Van\r\nDorn's retreating column just as it was crossing the Hatchie by a\r\nbridge some ten miles out from Corinth. The bottom land here was\r\nswampy and bad for the operations of troops, making a good place to\r\nget an enemy into. Ord attacked the troops that had crossed the\r\nbridge and drove them back in a panic. Many were killed, and others\r\nwere drowned by being pushed off the bridge in their hurried\r\nretreat. Ord followed and met the main force. He was too weak in\r\nnumbers to assault, but he held the bridge and compelled the enemy\r\nto resume his retreat by another bridge higher up the stream. Ord\r\nwas wounded in this engagement and the command devolved on\r\nHurlbut.", 'Paragraph 293': "Rosecrans did not start in pursuit till the morning of the 5th\r\nand then took the wrong road. Moving in the enemy's country he\r\ntravelled with a wagon train to carry his provisions and munitions\r\nof war. His march was therefore slower than that of the enemy, who\r\nwas moving towards his supplies. Two or three hours of pursuit on\r\nthe day of battle, without anything except what the men carried on\r\ntheir persons, would have been worth more than any pursuit\r\ncommenced the next day could have possibly been. Even when he did\r\nstart, if Rosecrans had followed the route taken by the enemy, he\r\nwould have come upon Van Dorn in a swamp with a stream in front and\r\nOrd holding the only bridge; but he took the road leading north and\r\ntowards Chewalla instead of west, and, after having marched as far\r\nas the enemy had moved to get to the Hatchie, he was as far from\r\nbattle as when he started. Hurlbut had not the numbers to meet any\r\nsuch force as Van Dorn's if they had been in any mood for fighting,\r\nand he might have been in great peril.", 'Paragraph 294': 'I now regarded the time to accomplish anything by pursuit as\r\npast and, after Rosecrans reached Jonesboro, I ordered him to\r\nreturn. He kept on to Ripley, however, and was persistent in\r\nwanting to go farther. I thereupon ordered him to halt and\r\nsubmitted the matter to the general-in-chief, who allowed me to\r\nexercise my judgment in the matter, but inquired "why not pursue?"\r\nUpon this I ordered Rosecrans back. Had he gone much farther he\r\nwould have met a greater force than Van Dorn had at Corinth and\r\nbehind intrenchments or on chosen ground, and the probabilities are\r\nhe would have lost his army.', 'Paragraph 295': 'The battle of Corinth was bloody, our loss being 315 killed,\r\n1,812 wounded and 232 missing. The enemy lost many more. Rosecrans\r\nreported 1,423 dead and 2,225 prisoners. We fought behind\r\nbreastworks, which accounts in some degree for the disparity. Among\r\nthe killed on our side was General Hackelman. General Oglesby was\r\nbadly, it was for some time supposed mortally, wounded. I received\r\na congratulatory letter from the President, which expressed also\r\nhis sorrow for the losses.', 'Paragraph 296': 'This battle was recognized by me as being a decided victory,\r\nthough not so complete as I had hoped for, nor nearly so complete\r\nas I now think was within the easy grasp of the commanding officer\r\nat Corinth. Since the war it is known that the result, as it was,\r\nwas a crushing blow to the enemy, and felt by him much more than it\r\nwas appreciated at the North. The battle relieved me from any\r\nfurther anxiety for the safety of the territory within my\r\njurisdiction, and soon after receiving reinforcements I suggested\r\nto the general-in-chief a forward movement against Vicksburg.', 'Paragraph 297': "On the 23d of October I learned of Pemberton's being in command\r\nat Holly Springs and much reinforced by conscripts and troops from\r\nAlabama and Texas. The same day General Rosecrans was relieved from\r\nduty with my command, and shortly after he succeeded Buell in the\r\ncommand of the army in Middle Tennessee. I was delighted at the\r\npromotion of General Rosecrans to a separate command, because I\r\nstill believed that when independent of an immediate superior the\r\nqualities which I, at that time, credited him with possessing,\r\nwould show themselves. As a subordinate I found that I could not\r\nmake him do as I wished, and had determined to relieve him from\r\nduty that very day.", 'Paragraph 298': 'At the close of the operations just described my force, in round\r\nnumbers, was 48,500. Of these 4,800 were in Kentucky and Illinois,\r\n7,000 in Memphis, 19,200 from Mound City south, and 17,500 at\r\nCorinth. General McClernand had been authorized from Washington to\r\ngo north and organize troops to be used in opening the Mississippi.\r\nThese new levies with other reinforcements now began to come\r\nin.', 'Paragraph 299': 'On the 25th of October I was placed in command of the Department\r\nof the Tennessee. Reinforcements continued to come from the north\r\nand by the 2d of November I was prepared to take the initiative.\r\nThis was a great relief after the two and a half months of\r\ncontinued defence over a large district of country, and where\r\nnearly every citizen was an enemy ready to give information of our\r\nevery move. I have described very imperfectly a few of the battles\r\nand skirmishes that took place during this time. To describe all\r\nwould take more space than I can allot to the purpose; to make\r\nspecial mention of all the officers and troops who distinguished\r\nthemselves, would take a volume.', 'Paragraph 300': '[NOTE.—For gallantry in the various engagements,\r\nfrom the time I was left in command down to 26th of October and on\r\nmy recommendation, Generals McPherson and C. S. Hamilton were\r\npromoted to be Major-Generals, and Colonels C. C. Marsh, 20th\r\nIllinois, M. M. Crocker, 13th Iowa J. A. Mower, 11th Missouri, M.\r\nD. Leggett, 78th Ohio, J. D. Stevenson, 7th Missouri, and John E.\r\nSmith, 45th Illinois, to be Brigadiers.]', 'Paragraph 301': 'Vicksburg was important to the enemy because it occupied the\r\nfirst high ground coming close to the river below Memphis. From\r\nthere a railroad runs east, connecting with other roads leading to\r\nall points of the Southern States. A railroad also starts from the\r\nopposite side of the river, extending west as far as Shreveport,\r\nLouisiana. Vicksburg was the only channel, at the time of the\r\nevents of which this chapter treats, connecting the parts of the\r\nConfederacy divided by the Mississippi. So long as it was held by\r\nthe enemy, the free navigation of the river was prevented. Hence\r\nits importance. Points on the river between Vicksburg and Port\r\nHudson were held as dependencies; but their fall was sure to follow\r\nthe capture of the former place.', 'Paragraph 302': 'The campaign against Vicksburg commenced on the 2d of November\r\nas indicated in a dispatch to the general-in-chief in the following\r\nwords: "I have commenced a movement on Grand Junction, with three\r\ndivisions from Corinth and two from Bolivar. Will leave here\r\n[Jackson, Tennessee] to-morrow, and take command in person. If\r\nfound practicable, I will go to Holly Springs, and, may be,\r\nGrenada, completing railroad and telegraph as I go."', 'Paragraph 303': 'At this time my command was holding the Mobile and Ohio railroad\r\nfrom about twenty-five miles south of Corinth, north to Columbus,\r\nKentucky; the Mississippi Central from Bolivar north to its\r\njunction with the Mobile and Ohio; the Memphis and Charleston from\r\nCorinth east to Bear Creek, and the Mississippi River from Cairo to\r\nMemphis. My entire command was no more than was necessary to hold\r\nthese lines, and hardly that if kept on the defensive. By moving\r\nagainst the enemy and into his unsubdued, or not yet captured,\r\nterritory, driving their army before us, these lines would nearly\r\nhold themselves; thus affording a large force for field operations.\r\nMy moving force at that time was about 30,000 men, and I estimated\r\nthe enemy confronting me, under Pemberton, at about the same\r\nnumber. General McPherson commanded my left wing and General C. S.\r\nHamilton the centre, while Sherman was at Memphis with the right\r\nwing. Pemberton was fortified at the Tallahatchie, but occupied\r\nHolly Springs and Grand Junction on the Mississippi Central\r\nrailroad. On the 8th we occupied Grand Junction and La Grange,\r\nthrowing a considerable force seven or eight miles south, along the\r\nline of the railroad. The road from Bolivar forward was repaired\r\nand put in running order as the troops advanced.', 'Paragraph 304': 'Up to this time it had been regarded as an axiom in war that\r\nlarge bodies of troops must operate from a base of supplies which\r\nthey always covered and guarded in all forward movements. There was\r\ndelay therefore in repairing the road back, and in gathering and\r\nforwarding supplies to the front.', 'Paragraph 305': "By my orders, and in accordance with previous instructions from\r\nWashington, all the forage within reach was collected under the\r\nsupervision of the chief quartermaster and the provisions under the\r\nchief commissary, receipts being given when there was any one to\r\ntake them; the supplies in any event to be accounted for as\r\ngovernment stores. The stock was bountiful, but still it gave me no\r\nidea of the possibility of supplying a moving column in an enemy's\r\ncountry from the country itself.", 'Paragraph 306': 'It was at this point, probably, where the first idea of a\r\n"Freedman\'s Bureau" took its origin. Orders of the government\r\nprohibited the expulsion of the negroes from the protection of the\r\narmy, when they came in voluntarily. Humanity forbade allowing them\r\nto starve. With such an army of them, of all ages and both sexes,\r\nas had congregated about Grand Junction, amounting to many\r\nthousands, it was impossible to advance. There was no special\r\nauthority for feeding them unless they were employed as teamsters,\r\ncooks and pioneers with the army; but only able-bodied young men\r\nwere suitable for such work. This labor would support but a very\r\nlimited percentage of them. The plantations were all deserted; the\r\ncotton and corn were ripe: men, women and children above ten years\r\nof age could be employed in saving these crops. To do this work\r\nwith contrabands, or to have it done, organization under a\r\ncompetent chief was necessary. On inquiring for such a man Chaplain\r\nEaton, now and for many years the very able United States\r\nCommissioner of Education, was suggested. He proved as efficient in\r\nthat field as he has since done in his present one. I gave him all\r\nthe assistants and guards he called for. We together fixed the\r\nprices to be paid for the negro labor, whether rendered to the\r\ngovernment or to individuals. The cotton was to be picked from\r\nabandoned plantations, the laborers to receive the stipulated price\r\n(my recollection is twelve and a half cents per pound for picking\r\nand ginning) from the quartermaster, he shipping the cotton north\r\nto be sold for the benefit of the government. Citizens remaining on\r\ntheir plantations were allowed the privilege of having their crops\r\nsaved by freedmen on the same terms.', 'Paragraph 307': 'At once the freedmen became self-sustaining. The money was not\r\npaid to them directly, but was expended judiciously and for their\r\nbenefit. They gave me no trouble afterwards.', 'Paragraph 308': 'Later the freedmen were engaged in cutting wood along the\r\nMississippi River to supply the large number of steamers on that\r\nstream. A good price was paid for chopping wood used for the supply\r\nof government steamers (steamers chartered and which the government\r\nhad to supply with fuel). Those supplying their own fuel paid a\r\nmuch higher price. In this way a fund was created not only\r\nsufficient to feed and clothe all, old and young, male and female,\r\nbut to build them comfortable cabins, hospitals for the sick, and\r\nto supply them with many comforts they had never known before.', 'Paragraph 309': 'At this stage of the campaign against Vicksburg I was very much\r\ndisturbed by newspaper rumors that General McClernand was to have a\r\nseparate and independent command within mine, to operate against\r\nVicksburg by way of the Mississippi River. Two commanders on the\r\nsame field are always one too many, and in this case I did not\r\nthink the general selected had either the experience or the\r\nqualifications to fit him for so important a position. I feared for\r\nthe safety of the troops intrusted to him, especially as he was to\r\nraise new levies, raw troops, to execute so important a trust. But\r\non the 12th I received a dispatch from General Halleck saying that\r\nI had command of all the troops sent to my department and\r\nauthorizing me to fight the enemy where I pleased. The next day my\r\ncavalry was in Holly Springs, and the enemy fell back south of the\r\nTallahatchie.', 'Paragraph 310': "Holly Springs I selected for my depot of supplies and munitions\r\nof war, all of which at that time came by rail from Columbus,\r\nKentucky, except the few stores collected about La Grange and Grand\r\nJunction. This was a long line (increasing in length as we moved\r\nsouth) to maintain in an enemy's country. On the 15th of November,\r\nwhile I was still at Holly Springs, I sent word to Sherman to meet\r\nme at Columbus. We were but forty-seven miles apart, yet the most\r\nexpeditious way for us to meet was for me to take the rail to\r\nColumbus and Sherman a steamer for the same place. At that meeting,\r\nbesides talking over my general plans I gave him his orders to join\r\nme with two divisions and to march them down the Mississippi\r\nCentral railroad if he could. Sherman, who was always prompt, was\r\nup by the 29th to Cottage Hill, ten miles north of Oxford. He\r\nbrought three divisions with him, leaving a garrison of only four\r\nregiments of infantry, a couple of pieces of artillery and a small\r\ndetachment of cavalry. Further reinforcements he knew were on their\r\nway from the north to Memphis. About this time General Halleck\r\nordered troops from Helena, Arkansas (territory west of the\r\nMississippi was not under my command then) to cut the road in\r\nPemberton's rear. The expedition was under Generals Hovey and C. C.\r\nWashburn and was successful so far as reaching the railroad was\r\nconcerned, but the damage done was very slight and was soon\r\nrepaired.", 'Paragraph 311': "The Tallahatchie, which confronted me, was very high, the\r\nrailroad bridge destroyed and Pemberton strongly fortified on the\r\nsouth side. A crossing would have been impossible in the presence\r\nof an enemy. I sent the cavalry higher up the stream and they\r\nsecured a crossing. This caused the enemy to evacuate their\r\nposition, which was possibly accelerated by the expedition of Hovey\r\nand Washburn. The enemy was followed as far south as Oxford by the\r\nmain body of troops, and some seventeen miles farther by\r\nMcPherson's command. Here the pursuit was halted to repair the\r\nrailroad from the Tallahatchie northward, in order to bring up\r\nsupplies. The piles on which the railroad bridge rested had been\r\nleft standing. The work of constructing a roadway for the troops\r\nwas but a short matter, and, later, rails were laid for cars.", 'Paragraph 312': 'During the delay at Oxford in repairing railroads I learned that\r\nan expedition down the Mississippi now was inevitable and, desiring\r\nto have a competent commander in charge, I ordered Sherman on the\r\n8th of December back to Memphis to take charge. The following were\r\nhis orders:', 'Paragraph 313': 'Headquarters 13th Army Corps,\r\nDepartment of the Tennessee.\r\nOXFORD, MISSISSIPPI, December 8,1862.', 'Paragraph 314': 'MAJOR-GENERAL W. T. SHERMAN,\r\nCommanding Right Wing:', 'Paragraph 315': "You will proceed, with as little delay as possible, to Memphis,\r\nTennessee, taking with you one division of your present command. On\r\nyour arrival at Memphis you will assume command of all the troops\r\nthere, and that portion of General Curtis's forces at present east\r\nof the Mississippi River, and organize them into brigades and\r\ndivisions in your own army. As soon as possible move with them down\r\nthe river to the vicinity of Vicksburg, and with the co-operation\r\nof the gunboat fleet under command of Flag-officer Porter proceed\r\nto the reduction of that place in such a manner as circumstances,\r\nand your own judgment, may dictate.", 'Paragraph 316': 'The amount of rations, forage, land transportation, etc.,\r\nnecessary to take, will be left entirely with yourself. The\r\nQuartermaster at St. Louis will be instructed to send you\r\ntransportation for 30,000 men; should you still find yourself\r\ndeficient, your quartermaster will be authorized to make up the\r\ndeficiency from such transports as may come into the port of\r\nMemphis.', 'Paragraph 317': 'On arriving in Memphis, put yourself in communication with\r\nAdmiral Porter, and arrange with him for his co-operation.', 'Paragraph 318': 'Inform me at the earliest practicable day of the time when you\r\nwill embark, and such plans as may then be matured. I will hold the\r\nforces here in readiness to co-operate with you in such manner as\r\nthe movements of the enemy may make necessary.', 'Paragraph 319': 'Leave the District of Memphis in the command of an efficient\r\nofficer, and with a garrison of four regiments of infantry, the\r\nsiege guns, and whatever cavalry may be there.', 'Paragraph 320': 'U. S. GRANT,\r\nMajor-General.', 'Paragraph 321': 'This idea had presented itself to my mind earlier, for on the 3d\r\nof December I asked Halleck if it would not be well to hold the\r\nenemy south of the Yallabusha and move a force from Helena and\r\nMemphis on Vicksburg. On the 5th again I suggested, from Oxford, to\r\nHalleck that if the Helena troops were at my command I though it\r\nwould be possible to take them and the Memphis forces south of the\r\nmouth of the Yazoo River, and thus secure Vicksburg and the State\r\nof Mississippi. Halleck on the same day, the 5th of December,\r\ndirected me not to attempt to hold the country south of the\r\nTallahatchie, but to collect 25,000 troops at Memphis by the 20th\r\nfor the Vicksburg expedition. I sent Sherman with two divisions at\r\nonce, informed the general-in-chief of the fact, and asked whether\r\nI should command the expedition down the river myself or send\r\nSherman. I was authorized to do as I though best for the\r\naccomplishment of the great object in view. I sent Sherman and so\r\ninformed General Halleck.', 'Paragraph 322': "As stated, my action in sending Sherman back was expedited by a\r\ndesire to get him in command of the forces separated from my direct\r\nsupervision. I feared that delay might bring McClernand, who was\r\nhis senior and who had authority from the President and Secretary\r\nof War to exercise that particular command,—and\r\nindependently. I doubted McClernand's fitness; and I had good\r\nreason to believe that in forestalling him I was by no means giving\r\noffence to those whose authority to command was above both him and\r\nme.", 'Paragraph 323': "Neither my orders to General Sherman, nor the correspondence\r\nbetween us or between General Halleck and myself, contemplated at\r\nthe time my going further south than the Yallabusha. Pemberton's\r\nforce in my front was the main part of the garrison of Vicksburg,\r\nas the force with me was the defence of the territory held by us in\r\nWest Tennessee and Kentucky. I hoped to hold Pemberton in my front\r\nwhile Sherman should get in his rear and into Vicksburg. The\r\nfurther north the enemy could be held the better.", 'Paragraph 324': 'It was understood, however, between General Sherman and myself\r\nthat our movements were to be co-operative; if Pemberton could not\r\nbe held away from Vicksburg I was to follow him; but at that time\r\nit was not expected to abandon the railroad north of the\r\nYallabusha. With that point as a secondary base of supplies, the\r\npossibility of moving down the Yazoo until communications could be\r\nopened with the Mississippi was contemplated.', 'Paragraph 325': "It was my intention, and so understood by Sherman and his\r\ncommand, that if the enemy should fall back I would follow him even\r\nto the gates of Vicksburg. I intended in such an event to hold the\r\nroad to Grenada on the Yallabusha and cut loose from there,\r\nexpecting to establish a new base of supplies on the Yazoo, or at\r\nVicksburg itself, with Grenada to fall back upon in case of\r\nfailure. It should be remembered that at the time I speak of it had\r\nnot been demonstrated that an army could operate in an enemy's\r\nterritory depending upon the country for supplies. A halt was\r\ncalled at Oxford with the advance seventeen miles south of there,\r\nto bring up the road to the latter point and to bring supplies of\r\nfood, forage and munitions to the front.", 'Paragraph 326': 'On the 18th of December I received orders from Washington to\r\ndivide my command into four army corps, with General McClernand to\r\ncommand one of them and to be assigned to that part of the army\r\nwhich was to operate down the Mississippi. This interfered with my\r\nplans, but probably resulted in my ultimately taking the command in\r\nperson. McClernand was at that time in Springfield, Illinois. The\r\norder was obeyed without any delay. Dispatches were sent to him the\r\nsame day in conformity.', 'Paragraph 327': "On the 20th General Van Dorn appeared at Holly Springs, my\r\nsecondary base of supplies, captured the garrison of 1,500 men\r\ncommanded by Colonel Murphy, of the 8th Wisconsin regiment, and\r\ndestroyed all our munitions of war, food and forage. The capture\r\nwas a disgraceful one to the officer commanding but not to the\r\ntroops under him. At the same time Forrest got on our line of\r\nrailroad between Jackson, Tennessee, and Columbus, Kentucky, doing\r\nmuch damage to it. This cut me off from all communication with the\r\nnorth for more than a week, and it was more than two weeks before\r\nrations or forage could be issued from stores obtained in the\r\nregular way. This demonstrated the impossibility of maintaining so\r\nlong a line of road over which to draw supplies for an army moving\r\nin an enemy's country. I determined, therefore, to abandon my\r\ncampaign into the interior with Columbus as a base, and returned to\r\nLa Grange and Grand Junction destroying the road to my front and\r\nrepairing the road to Memphis, making the Mississippi river the\r\nline over which to draw supplies. Pemberton was falling back at the\r\nsame time.", 'Paragraph 328': "The moment I received the news of Van Dorn's success I sent the\r\ncavalry at the front back to drive him from the country. He had\r\nstart enough to move north destroying the railroad in many places,\r\nand to attack several small garrisons intrenched as guards to the\r\nrailroad. All these he found warned of his coming and prepared to\r\nreceive him. Van Dorn did not succeed in capturing a single\r\ngarrison except the one at Holly Springs, which was larger than all\r\nthe others attacked by him put together. Murphy was also warned of\r\nVan Dorn's approach, but made no preparations to meet him. He did\r\nnot even notify his command.", 'Paragraph 329': "Colonel Murphy was the officer who, two months before, had\r\nevacuated Iuka on the approach of the enemy. General Rosecrans\r\ndenounced him for the act and desired to have him tried and\r\npunished. I sustained the colonel at the time because his command\r\nwas a small one compared with that of the enemy—not one-tenth\r\nas large—and I thought he had done well to get away without\r\nfalling into their hands. His leaving large stores to fall into\r\nPrice's possession I looked upon as an oversight and excused it on\r\nthe ground of inexperience in military matters. He should, however,\r\nhave destroyed them. This last surrender demonstrated to my mind\r\nthat Rosecrans' judgment of Murphy's conduct at Iuka was correct.\r\nThe surrender of Holly Springs was most reprehensible and showed\r\neither the disloyalty of Colonel Murphy to the cause which he\r\nprofessed to serve, or gross cowardice.", 'Paragraph 330': 'After the war was over I read from the diary of a lady who\r\naccompanied General Pemberton in his retreat from the Tallahatchie,\r\nthat the retreat was almost a panic. The roads were bad and it was\r\ndifficult to move the artillery and trains. Why there should have\r\nbeen a panic I do not see. No expedition had yet started down the\r\nMississippi River. Had I known the demoralized condition of the\r\nenemy, or the fact that central Mississippi abounded so in all army\r\nsupplies, I would have been in pursuit of Pemberton while his\r\ncavalry was destroying the roads in my rear.', 'Paragraph 331': "After sending cavalry to drive Van Dorn away, my next order was\r\nto dispatch all the wagons we had, under proper escort, to collect\r\nand bring in all supplies of forage and food from a region of\r\nfifteen miles east and west of the road from our front back to\r\nGrand Junction, leaving two months' supplies for the families of\r\nthose whose stores were taken. I was amazed at the quantity of\r\nsupplies the country afforded. It showed that we could have\r\nsubsisted off the country for two months instead of two weeks\r\nwithout going beyond the limits designated. This taught me a lesson\r\nwhich was taken advantage of later in the campaign when our army\r\nlived twenty days with the issue of only five days' rations by the\r\ncommissary. Our loss of supplies was great at Holly Springs, but it\r\nwas more than compensated for by those taken from the country and\r\nby the lesson taught.", 'Paragraph 332': 'The news of the capture of Holly Springs and the destruction of\r\nour supplies caused much rejoicing among the people remaining in\r\nOxford. They came with broad smiles on their faces, indicating\r\nintense joy, to ask what I was going to do now without anything for\r\nmy soldiers to eat. I told them that I was not disturbed; that I\r\nhad already sent troops and wagons to collect all the food and\r\nforage they could find for fifteen miles on each side of the road.\r\nCountenances soon changed, and so did the inquiry. The next was,\r\n"What are WE to do?" My response was that we had endeavored to feed\r\nourselves from our own northern resources while visiting them; but\r\ntheir friends in gray had been uncivil enough to destroy what we\r\nhad brought along, and it could not be expected that men, with arms\r\nin their hands, would starve in the midst of plenty. I advised them\r\nto emigrate east, or west, fifteen miles and assist in eating up\r\nwhat we left.', 'Paragraph 333': "This interruption in my communications north—I was really\r\ncut off from communication with a great part of my own command\r\nduring this time—resulted in Sherman's moving from Memphis\r\nbefore McClernand could arrive, for my dispatch of the 18th did not\r\nreach McClernand. Pemberton got back to Vicksburg before Sherman\r\ngot there. The rebel positions were on a bluff on the Yazoo River,\r\nsome miles above its mouth. The waters were high so that the\r\nbottoms were generally overflowed, leaving only narrow causeways of\r\ndry land between points of debarkation and the high bluffs. These\r\nwere fortified and defended at all points. The rebel position was\r\nimpregnable against any force that could be brought against its\r\nfront. Sherman could not use one-fourth of his force. His efforts\r\nto capture the city, or the high ground north of it, were\r\nnecessarily unavailing.", 'Paragraph 334': "Sherman's attack was very unfortunate, but I had no opportunity\r\nof communicating with him after the destruction of the road and\r\ntelegraph to my rear on the 20th. He did not know but what I was in\r\nthe rear of the enemy and depending on him to open a new base of\r\nsupplies for the troops with me. I had, before he started from\r\nMemphis, directed him to take with him a few small steamers\r\nsuitable for the navigation of the Yazoo, not knowing but that I\r\nmight want them to supply me after cutting loose from my base at\r\nGrenada.", 'Paragraph 335': 'On the 23d I removed my headquarters back to Holly Springs. The\r\ntroops were drawn back gradually, but without haste or confusion,\r\nfinding supplies abundant and no enemy following. The road was not\r\ndamaged south of Holly Springs by Van Dorn, at least not to an\r\nextent to cause any delay. As I had resolved to move headquarters\r\nto Memphis, and to repair the road to that point, I remained at\r\nHolly Springs until this work was completed.', 'Paragraph 336': 'On the 10th of January, the work on the road from Holly Springs\r\nto Grand Junction and thence to Memphis being completed, I moved my\r\nheadquarters to the latter place. During the campaign here\r\ndescribed, the losses (mostly captures) were about equal, crediting\r\nthe rebels with their Holly Springs capture, which they could not\r\nhold.', 'Paragraph 337': "When Sherman started on his expedition down the river he had\r\n20,000 men, taken from Memphis, and was reinforced by 12,000 more\r\nat Helena, Arkansas. The troops on the west bank of the river had\r\npreviously been assigned to my command. McClernand having received\r\nthe orders for his assignment reached the mouth of the Yazoo on the\r\n2d of January, and immediately assumed command of all the troops\r\nwith Sherman, being a part of his own corps, the 13th, and all of\r\nSherman's, the 15th. Sherman, and Admiral Porter with the fleet,\r\nhad withdrawn from the Yazoo. After consultation they decided that\r\nneither the army nor navy could render service to the cause where\r\nthey were, and learning that I had withdrawn from the interior of\r\nMississippi, they determined to return to the Arkansas River and to\r\nattack Arkansas Post, about fifty miles up that stream and\r\ngarrisoned by about five or six thousand men. Sherman had learned\r\nof the existence of this force through a man who had been captured\r\nby the enemy with a steamer loaded with ammunition and other\r\nsupplies intended for his command. The man had made his escape.\r\nMcClernand approved this move reluctantly, as Sherman says. No\r\nobstacle was encountered until the gunboats and transports were\r\nwithin range of the fort. After three days' bombardment by the navy\r\nan assault was made by the troops and marines, resulting in the\r\ncapture of the place, and in taking 5,000 prisoners and 17 guns. I\r\nwas at first disposed to disapprove of this move as an unnecessary\r\nside movement having no especial bearing upon the work before us;\r\nbut when the result was understood I regarded it as very important.\r\nFive thousand Confederate troops left in the rear might have caused\r\nus much trouble and loss of property while navigating the\r\nMississippi.", 'Paragraph 338': "Immediately after the reduction of Arkansas Post and the capture\r\nof the garrison, McClernand returned with his entire force to\r\nNapoleon, at the mouth of the Arkansas River. From here I received\r\nmessages from both Sherman and Admiral Porter, urging me to come\r\nand take command in person, and expressing their distrust of\r\nMcClernand's ability and fitness for so important and intricate an\r\nexpedition.", 'Paragraph 339': "On the 17th I visited McClernand and his command at Napoleon. It\r\nwas here made evident to me that both the army and navy were so\r\ndistrustful of McClernand's fitness to command that, while they\r\nwould do all they could to insure success, this distrust was an\r\nelement of weakness. It would have been criminal to send troops\r\nunder these circumstances into such danger. By this time I had\r\nreceived authority to relieve McClernand, or to assign any person\r\nelse to the command of the river expedition, or to assume command\r\nin person. I felt great embarrassment about McClernand. He was the\r\nsenior major-general after myself within the department. It would\r\nnot do, with his rank and ambition, to assign a junior over him.\r\nNothing was left, therefore, but to assume the command myself. I\r\nwould have been glad to put Sherman in command, to give him an\r\nopportunity to accomplish what he had failed in the December\r\nbefore; but there seemed no other way out of the difficulty, for he\r\nwas junior to McClernand. Sherman's failure needs no apology.", 'Paragraph 340': "On the 20th I ordered General McClernand with the entire\r\ncommand, to Young's Point and Milliken's Bend, while I returned to\r\nMemphis to make all the necessary preparation for leaving the\r\nterritory behind me secure. General Hurlbut with the 16th corps was\r\nleft in command. The Memphis and Charleston railroad was held,\r\nwhile the Mississippi Central was given up. Columbus was the only\r\npoint between Cairo and Memphis, on the river, left with a\r\ngarrison. All the troops and guns from the posts on the abandoned\r\nrailroad and river were sent to the front.", 'Paragraph 341': "On the 29th of January I arrived at Young's Point and assumed\r\ncommand the following day. General McClernand took exception in a\r\nmost characteristic way—for him. His correspondence with me\r\non the subject was more in the nature of a reprimand than a\r\nprotest. It was highly insubordinate, but I overlooked it, as I\r\nbelieved, for the good of the service. General McClernand was a\r\npolitician of very considerable prominence in his State; he was a\r\nmember of Congress when the secession war broke out; he belonged to\r\nthat political party which furnished all the opposition there was\r\nto a vigorous prosecution of the war for saving the Union; there\r\nwas no delay in his declaring himself for the Union at all hazards,\r\nand there was no uncertain sound in his declaration of where he\r\nstood in the contest before the country. He also gave up his seat\r\nin Congress to take the field in defence of the principles he had\r\nproclaimed.", 'Paragraph 342': "The real work of the campaign and siege of Vicksburg now began.\r\nThe problem was to secure a footing upon dry ground on the east\r\nside of the river from which the troops could operate against\r\nVicksburg. The Mississippi River, from Cairo south, runs through a\r\nrich alluvial valley of many miles in width, bound on the east by\r\nland running from eighty up to two or more hundred feet above the\r\nriver. On the west side the highest land, except in a few places,\r\nis but little above the highest water. Through this valley the\r\nriver meanders in the most tortuous way, varying in direction to\r\nall points of the compass. At places it runs to the very foot of\r\nthe bluffs. After leaving Memphis, there are no such highlands\r\ncoming to the water's edge on the east shore until Vicksburg is\r\nreached.", 'Paragraph 343': "The intervening land is cut up by bayous filled from the river\r\nin high water—many of them navigable for steamers. All of\r\nthem would be, except for overhanging trees, narrowness and\r\ntortuous course, making it impossible to turn the bends with\r\nvessels of any considerable length. Marching across this country in\r\nthe face of an enemy was impossible; navigating it proved equally\r\nimpracticable. The strategical way according to the rule,\r\ntherefore, would have been to go back to Memphis; establish that as\r\na base of supplies; fortify it so that the storehouses could be\r\nheld by a small garrison, and move from there along the line of\r\nrailroad, repairing as we advanced, to the Yallabusha, or to\r\nJackson, Mississippi. At this time the North had become very much\r\ndiscouraged. Many strong Union men believed that the war must prove\r\na failure. The elections of 1862 had gone against the party which\r\nwas for the prosecution of the war to save the Union if it took the\r\nlast man and the last dollar. Voluntary enlistments had ceased\r\nthroughout the greater part of the North, and the draft had been\r\nresorted to to fill up our ranks. It was my judgment at the time\r\nthat to make a backward movement as long as that from Vicksburg to\r\nMemphis, would be interpreted, by many of those yet full of hope\r\nfor the preservation of the Union, as a defeat, and that the draft\r\nwould be resisted, desertions ensue and the power to capture and\r\npunish deserters lost. There was nothing left to be done but to go\r\nFORWARD TO A DECISIVE VICTORY. This was in my mind from the moment\r\nI took command in person at Young's Point.", 'Paragraph 344': 'The winter of 1862-3 was a noted one for continuous high water\r\nin the Mississippi and for heavy rains along the lower river. To\r\nget dry land, or rather land above the water, to encamp the troops\r\nupon, took many miles of river front. We had to occupy the levees\r\nand the ground immediately behind. This was so limited that one\r\ncorps, the 17th, under General McPherson, was at Lake Providence,\r\nseventy miles above Vicksburg.', 'Paragraph 345': 'It was in January the troops took their position opposite\r\nVicksburg. The water was very high and the rains were incessant.\r\nThere seemed no possibility of a land movement before the end of\r\nMarch or later, and it would not do to lie idle all this time. The\r\neffect would be demoralizing to the troops and injurious to their\r\nhealth. Friends in the North would have grown more and more\r\ndiscouraged, and enemies in the same section more and more insolent\r\nin their gibes and denunciation of the cause and those engaged in\r\nit.', 'Paragraph 346': 'I always admired the South, as bad as I thought their cause, for\r\nthe boldness with which they silenced all opposition and all\r\ncroaking, by press or by individuals, within their control. War at\r\nall times, whether a civil war between sections of a common country\r\nor between nations, ought to be avoided, if possible with honor.\r\nBut, once entered into, it is too much for human nature to tolerate\r\nan enemy within their ranks to give aid and comfort to the armies\r\nof the opposing section or nation.', 'Paragraph 347': "Vicksburg, as stated before, is on the first high land coming to\r\nthe river's edge, below that on which Memphis stands. The bluff, or\r\nhigh land, follows the left bank of the Yazoo for some distance and\r\ncontinues in a southerly direction to the Mississippi River, thence\r\nit runs along the Mississippi to Warrenton, six miles below. The\r\nYazoo River leaves the high land a short distance below Haines'\r\nBluff and empties into the Mississippi nine miles above Vicksburg.\r\nVicksburg is built on this high land where the Mississippi washes\r\nthe base of the hill. Haines' Bluff, eleven miles from Vicksburg,\r\non the Yazoo River, was strongly fortified. The whole distance from\r\nthere to Vicksburg and thence to Warrenton was also intrenched,\r\nwith batteries at suitable distances and rifle-pits connecting\r\nthem.", 'Paragraph 348': "From Young's Point the Mississippi turns in a north-easterly\r\ndirection to a point just above the city, when it again turns and\r\nruns south-westerly, leaving vessels, which might attempt to run\r\nthe blockade, exposed to the fire of batteries six miles below the\r\ncity before they were in range of the upper batteries. Since then\r\nthe river has made a cut-off, leaving what was the peninsula in\r\nfront of the city, an island. North of the Yazoo was all a marsh,\r\nheavily timbered, cut up with bayous, and much overflowed. A front\r\nattack was therefore impossible, and was never contemplated;\r\ncertainly not by me. The problem then became, how to secure a\r\nlanding on high ground east of the Mississippi without an apparent\r\nretreat. Then commenced a series of experiments to consume time,\r\nand to divert the attention of the enemy, of my troops and of the\r\npublic generally. I, myself, never felt great confidence that any\r\nof the experiments resorted to would prove successful. Nevertheless\r\nI was always prepared to take advantage of them in case they\r\ndid.", 'Paragraph 349': "In 1862 General Thomas Williams had come up from New Orleans and\r\ncut a ditch ten or twelve feet wide and about as deep, straight\r\nacross from Young's Point to the river below. The distance across\r\nwas a little over a mile. It was Williams' expectation that when\r\nthe river rose it would cut a navigable channel through; but the\r\ncanal started in an eddy from both ends, and, of course, it only\r\nfilled up with water on the rise without doing any execution in the\r\nway of cutting. Mr. Lincoln had navigated the Mississippi in his\r\nyounger days and understood well its tendency to change its\r\nchannel, in places, from time to time. He set much store\r\naccordingly by this canal. General McClernand had been, therefore,\r\ndirected before I went to Young's Point to push the work of\r\nwidening and deepening this canal. After my arrival the work was\r\ndiligently pushed with about 4,000 men—as many as could be\r\nused to advantage—until interrupted by a sudden rise in the\r\nriver that broke a dam at the upper end, which had been put there\r\nto keep the water out until the excavation was completed. This was\r\non the 8th of March.", 'Paragraph 350': 'Even if the canal had proven a success, so far as to be\r\nnavigable for steamers, it could not have been of much advantage to\r\nus. It runs in a direction almost perpendicular to the line of\r\nbluffs on the opposite side, or east bank, of the river. As soon as\r\nthe enemy discovered what we were doing he established a battery\r\ncommanding the canal throughout its length. This battery soon drove\r\nout our dredges, two in number, which were doing the work of\r\nthousands of men. Had the canal been completed it might have proven\r\nof some use in running transports through, under the cover of\r\nnight, to use below; but they would yet have to run batteries,\r\nthough for a much shorter distance.', 'Paragraph 351': 'While this work was progressing we were busy in other\r\ndirections, trying to find an available landing on high ground on\r\nthe east bank of the river, or to make water-ways to get below the\r\ncity, avoiding the batteries.', 'Paragraph 352': 'On the 30th of January, the day after my arrival at the front, I\r\nordered General McPherson, stationed with his corps at Lake\r\nProvidence, to cut the levee at that point. If successful in\r\nopening a channel for navigation by this route, it would carry us\r\nto the Mississippi River through the mouth of the Red River, just\r\nabove Port Hudson and four hundred miles below Vicksburg by the\r\nriver.', 'Paragraph 353': 'Lake Providence is a part of the old bed of the Mississippi,\r\nabout a mile from the present channel. It is six miles long and has\r\nits outlet through Bayou Baxter, Bayou Macon, and the Tensas,\r\nWashita and Red Rivers. The last three are navigable streams at all\r\nseasons. Bayous Baxter and Macon are narrow and tortuous, and the\r\nbanks are covered with dense forests overhanging the channel. They\r\nwere also filled with fallen timber, the accumulation of years. The\r\nland along the Mississippi River, from Memphis down, is in all\r\ninstances highest next to the river, except where the river washes\r\nthe bluffs which form the boundary of the valley through which it\r\nwinds. Bayou Baxter, as it reaches lower land, begins to spread out\r\nand disappears entirely in a cypress swamp before it reaches the\r\nMacon. There was about two feet of water in this swamp at the time.\r\nTo get through it, even with vessels of the lightest draft, it was\r\nnecessary to clear off a belt of heavy timber wide enough to make a\r\npassage way. As the trees would have to be cut close to the\r\nbottom—under water—it was an undertaking of great\r\nmagnitude.', 'Paragraph 354': "On the 4th of February I visited General McPherson, and remained\r\nwith him several days. The work had not progressed so far as to\r\nadmit the water from the river into the lake, but the troops had\r\nsucceeded in drawing a small steamer, of probably not over thirty\r\ntons' capacity, from the river into the lake. With this we were\r\nable to explore the lake and bayou as far as cleared. I saw then\r\nthat there was scarcely a chance of this ever becoming a\r\npracticable route for moving troops through an enemy's country. The\r\ndistance from Lake Providence to the point where vessels going by\r\nthat route would enter the Mississippi again, is about four hundred\r\nand seventy miles by the main river. The distance would probably be\r\ngreater by the tortuous bayous through which this new route would\r\ncarry us. The enemy held Port Hudson, below where the Red River\r\ndebouches, and all the Mississippi above to Vicksburg. The Red\r\nRiver, Washita and Tensas were, as has been said, all navigable\r\nstreams, on which the enemy could throw small bodies of men to\r\nobstruct our passage and pick off our troops with their\r\nsharpshooters. I let the work go on, believing employment was\r\nbetter than idleness for the men. Then, too, it served as a cover\r\nfor other efforts which gave a better prospect of success. This\r\nwork was abandoned after the canal proved a failure.", 'Paragraph 355': 'Lieutenant-Colonel Wilson of my staff was sent to Helena,\r\nArkansas, to examine and open a way through Moon Lake and the Yazoo\r\nPass if possible. Formerly there was a route by way of an inlet\r\nfrom the Mississippi River into Moon Lake, a mile east of the\r\nriver, thence east through Yazoo Pass to Coldwater, along the\r\nlatter to the Tallahatchie, which joins the Yallabusha about two\r\nhundred and fifty miles below Moon Lake and forms the Yazoo River.\r\nThese were formerly navigated by steamers trading with the rich\r\nplantations along their banks; but the State of Mississippi had\r\nbuilt a strong levee across the inlet some years before, leaving\r\nthe only entrance for vessels into this rich region the one by way\r\nof the mouth of the Yazoo several hundreds of miles below.', 'Paragraph 356': 'On the 2d of February this dam, or levee, was cut. The river\r\nbeing high the rush of water through the cut was so great that in a\r\nvery short time the entire obstruction was washed away. The bayous\r\nwere soon filled and much of the country was overflowed. This pass\r\nleaves the Mississippi River but a few miles below Helena. On the\r\n24th General Ross, with his brigade of about 4,500 men on\r\ntransports, moved into this new water-way. The rebels had\r\nobstructed the navigation of Yazoo Pass and the Coldwater by\r\nfelling trees into them. Much of the timber in this region being of\r\ngreater specific gravity than water, and being of great size, their\r\nremoval was a matter of great labor; but it was finally\r\naccomplished, and on the 11th of March Ross found himself,\r\naccompanied by two gunboats under the command of\r\nLieutenant-Commander Watson Smith, confronting a fortification at\r\nGreenwood, where the Tallahatchie and Yallabusha unite and the\r\nYazoo begins. The bends of the rivers are such at this point as to\r\nalmost form an island, scarcely above water at that stage of the\r\nriver. This island was fortified and manned. It was named Fort\r\nPemberton after the commander at Vicksburg. No land approach was\r\naccessible. The troops, therefore, could render no assistance\r\ntowards an assault further than to establish a battery on a little\r\npiece of ground which was discovered above water. The gunboats,\r\nhowever, attacked on the 11th and again on the 13th of March. Both\r\nefforts were failures and were not renewed. One gunboat was\r\ndisabled and we lost six men killed and twenty-five wounded. The\r\nloss of the enemy was less.', 'Paragraph 357': 'Fort Pemberton was so little above the water that it was thought\r\nthat a rise of two feet would drive the enemy out. In hope of\r\nenlisting the elements on our side, which had been so much against\r\nus up to this time, a second cut was made in the Mississippi levee,\r\nthis time directly opposite Helena, or six miles above the former\r\ncut. It did not accomplish the desired result, and Ross, with his\r\nfleet, started back. On the 22d he met Quinby with a brigade at\r\nYazoo Pass. Quinby was the senior of Ross, and assumed command. He\r\nwas not satisfied with returning to his former position without\r\nseeing for himself whether anything could be accomplished.\r\nAccordingly Fort Pemberton was revisited by our troops; but an\r\ninspection was sufficient this time without an attack. Quinby, with\r\nhis command, returned with but little delay. In the meantime I was\r\nmuch exercised for the safety of Ross, not knowing that Quinby had\r\nbeen able to join him. Reinforcements were of no use in a country\r\ncovered with water, as they would have to remain on board of their\r\ntransports. Relief had to come from another quarter. So I\r\ndetermined to get into the Yazoo below Fort Pemberton.', 'Paragraph 358': "Steel's Bayou empties into the Yazoo River between Haines' Bluff\r\nand its mouth. It is narrow, very tortuous, and fringed with a very\r\nheavy growth of timber, but it is deep. It approaches to within one\r\nmile of the Mississippi at Eagle Bend, thirty miles above Young's\r\nPoint. Steel's Bayou connects with Black Bayou, Black Bayou with\r\nDeer Creek, Deer Creek with Rolling Fork, Rolling Fork with the Big\r\nSunflower River, and the Big Sunflower with the Yazoo River about\r\nten miles above Haines' Bluff in a right line but probably twenty\r\nor twenty-five miles by the winding of the river. All these\r\nwaterways are of about the same nature so far as navigation is\r\nconcerned, until the Sunflower is reached; this affords free\r\nnavigation.", 'Paragraph 359': "Admiral Porter explored this waterway as far as Deer Creek on\r\nthe 14th of March, and reported it navigable. On the next day he\r\nstarted with five gunboats and four mortar-boats. I went with him\r\nfor some distance. The heavy overhanging timber retarded progress\r\nvery much, as did also the short turns in so narrow a stream. The\r\ngunboats, however, ploughed their way through without other damage\r\nthan to their appearance. The transports did not fare so well\r\nalthough they followed behind. The road was somewhat cleared for\r\nthem by the gunboats. In the evening I returned to headquarters to\r\nhurry up reinforcements. Sherman went in person on the 16th, taking\r\nwith him Stuart's division of the 15th corps. They took large river\r\ntransports to Eagle Bend on the Mississippi, where they debarked\r\nand marched across to Steel's Bayou, where they re-embarked on the\r\ntransports. The river steamers, with their tall smokestacks and\r\nlight guards extending out, were so much impeded that the gunboats\r\ngot far ahead. Porter, with his fleet, got within a few hundred\r\nyards of where the sailing would have been clear and free from the\r\nobstructions caused by felling trees into the water, when he\r\nencountered rebel sharp-shooters, and his progress was delayed by\r\nobstructions in his front. He could do nothing with gunboats\r\nagainst sharpshooters. The rebels, learning his route, had sent in\r\nabout 4,000 men—many more than there were sailors in the\r\nfleet.", 'Paragraph 360': 'Sherman went back, at the request of the admiral, to clear out\r\nBlack Bayou and to hurry up reinforcements, which were far behind.\r\nOn the night of the 19th he received notice from the admiral that\r\nhe had been attacked by sharp-shooters and was in imminent peril.\r\nSherman at once returned through Black Bayou in a canoe, and passed\r\non until he met a steamer, with the last of the reinforcements he\r\nhad, coming up. They tried to force their way through Black Bayou\r\nwith their steamer, but, finding it slow and tedious work, debarked\r\nand pushed forward on foot. It was night when they landed, and\r\nintensely dark. There was but a narrow strip of land above water,\r\nand that was grown up with underbrush or cane. The troops lighted\r\ntheir way through this with candles carried in their hands for a\r\nmile and a half, when they came to an open plantation. Here the\r\ntroops rested until morning. They made twenty-one miles from this\r\nresting-place by noon the next day, and were in time to rescue the\r\nfleet. Porter had fully made up his mind to blow up the gunboats\r\nrather than have them fall into the hands of the enemy. More\r\nwelcome visitors he probably never met than the "boys in blue" on\r\nthis occasion. The vessels were backed out and returned to their\r\nrendezvous on the Mississippi; and thus ended in failure the fourth\r\nattempt to get in rear of Vicksburg.', 'Paragraph 361': 'The original canal scheme was also abandoned on the 27th of\r\nMarch. The effort to make a waterway through Lake Providence and\r\nthe connecting bayous was abandoned as wholly impracticable about\r\nthe same time.', 'Paragraph 362': "At Milliken's Bend, and also at Young's Point, bayous or\r\nchannels start, which connecting with other bayous passing\r\nRichmond, Louisiana, enter the Mississippi at Carthage twenty-five\r\nor thirty miles above Grand Gulf. The Mississippi levee cuts the\r\nsupply of water off from these bayous or channels, but all the\r\nrainfall behind the levee, at these points, is carried through\r\nthese same channels to the river below. In case of a crevasse in\r\nthis vicinity, the water escaping would find its outlet through the\r\nsame channels. The dredges and laborers from the canal having been\r\ndriven out by overflow and the enemy's batteries, I determined to\r\nopen these other channels, if possible. If successful the effort\r\nwould afford a route, away from the enemy's batteries, for our\r\ntransports. There was a good road back of the levees, along these\r\nbayous, to carry the troops, artillery and wagon trains over\r\nwhenever the water receded a little, and after a few days of dry\r\nweather. Accordingly, with the abandonment of all the other plans\r\nfor reaching a base heretofore described, this new one was\r\nundertaken.", 'Paragraph 363': 'As early as the 4th of February I had written to Halleck about\r\nthis route, stating that I thought it much more practicable than\r\nthe other undertaking (the Lake Providence route), and that it\r\nwould have been accomplished with much less labor if commenced\r\nbefore the water had got all over the country.', 'Paragraph 364': 'The upper end of these bayous being cut off from a water supply,\r\nfurther than the rainfall back of the levees, was grown up with\r\ndense timber for a distance of several miles from their source. It\r\nwas necessary, therefore, to clear this out before letting in the\r\nwater from the river. This work was continued until the waters of\r\nthe river began to recede and the road to Richmond, Louisiana,\r\nemerged from the water. One small steamer and some barges were got\r\nthrough this channel, but no further use could be made of it\r\nbecause of the fall in the river. Beyond this it was no more\r\nsuccessful than the other experiments with which the winter was\r\nwhiled away. All these failures would have been very discouraging\r\nif I had expected much from the efforts; but I had not. From the\r\nfirst the most I hoped to accomplish was the passage of transports,\r\nto be used below Vicksburg, without exposure to the long line of\r\nbatteries defending that city.', 'Paragraph 365': 'This long, dreary and, for heavy and continuous rains and high\r\nwater, unprecedented winter was one of great hardship to all\r\nengaged about Vicksburg. The river was higher than its natural\r\nbanks from December, 1862, to the following April. The war had\r\nsuspended peaceful pursuits in the South, further than the\r\nproduction of army supplies, and in consequence the levees were\r\nneglected and broken in many places and the whole country was\r\ncovered with water. Troops could scarcely find dry ground on which\r\nto pitch their tents. Malarial fevers broke out among the men.\r\nMeasles and small-pox also attacked them. The hospital arrangements\r\nand medical attendance were so perfect, however, that the loss of\r\nlife was much less than might have been expected. Visitors to the\r\ncamps went home with dismal stories to relate; Northern papers came\r\nback to the soldiers with these stories exaggerated. Because I\r\nwould not divulge my ultimate plans to visitors, they pronounced me\r\nidle, incompetent and unfit to command men in an emergency, and\r\nclamored for my removal. They were not to be satisfied, many of\r\nthem, with my simple removal, but named who my successor should be.\r\nMcClernand, Fremont, Hunter and McClellan were all mentioned in\r\nthis connection. I took no steps to answer these complaints, but\r\ncontinued to do my duty, as I understood it, to the best of my\r\nability. Every one has his superstitions. One of mine is that in\r\npositions of great responsibility every one should do his duty to\r\nthe best of his ability where assigned by competent authority,\r\nwithout application or the use of influence to change his position.\r\nWhile at Cairo I had watched with very great interest the\r\noperations of the Army of the Potomac, looking upon that as the\r\nmain field of the war. I had no idea, myself, of ever having any\r\nlarge command, nor did I suppose that I was equal to one; but I had\r\nthe vanity to think that as a cavalry officer I might succeed very\r\nwell in the command of a brigade. On one occasion, in talking about\r\nthis to my staff officers, all of whom were civilians without any\r\nmilitary education whatever, I said that I would give anything if I\r\nwere commanding a brigade of cavalry in the Army of the Potomac and\r\nI believed I could do some good. Captain Hillyer spoke up and\r\nsuggested that I make application to be transferred there to\r\ncommand the cavalry. I then told him that I would cut my right arm\r\noff first, and mentioned this superstition.', 'Paragraph 366': 'In time of war the President, being by the Constitution\r\nCommander-in-chief of the Army and Navy, is responsible for the\r\nselection of commanders. He should not be embarrassed in making his\r\nselections. I having been selected, my responsibility ended with my\r\ndoing the best I knew how. If I had sought the place, or obtained\r\nit through personal or political influence, my belief is that I\r\nwould have feared to undertake any plan of my own conception, and\r\nwould probably have awaited direct orders from my distant\r\nsuperiors. Persons obtaining important commands by application or\r\npolitical influence are apt to keep a written record of complaints\r\nand predictions of defeat, which are shown in case of disaster.\r\nSomebody must be responsible for their failures.', 'Paragraph 367': 'With all the pressure brought to bear upon them, both President\r\nLincoln and General Halleck stood by me to the end of the campaign.\r\nI had never met Mr. Lincoln, but his support was constant.', 'Paragraph 368': "At last the waters began to recede; the roads crossing the\r\npeninsula behind the levees of the bayous, were emerging from the\r\nwaters; the troops were all concentrated from distant points at\r\nMilliken's Bend preparatory to a final move which was to crown the\r\nlong, tedious and discouraging labors with success.", 'Paragraph 369': "I had had in contemplation the whole winter the movement by land\r\nto a point below Vicksburg from which to operate, subject only to\r\nthe possible but not expected success of some one of the expedients\r\nresorted to for the purpose of giving us a different base. This\r\ncould not be undertaken until the waters receded. I did not\r\ntherefore communicate this plan, even to an officer of my staff,\r\nuntil it was necessary to make preparations for the start. My\r\nrecollection is that Admiral Porter was the first one to whom I\r\nmentioned it. The co-operation of the navy was absolutely essential\r\nto the success (even to the contemplation) of such an enterprise. I\r\nhad no more authority to command Porter than he had to command me.\r\nIt was necessary to have part of his fleet below Vicksburg if the\r\ntroops went there. Steamers to use as ferries were also essential.\r\nThe navy was the only escort and protection for these steamers, all\r\nof which in getting below had to run about fourteen miles of\r\nbatteries. Porter fell into the plan at once, and suggested that he\r\nhad better superintend the preparation of the steamers selected to\r\nrun the batteries, as sailors would probably understand the work\r\nbetter than soldiers. I was glad to accept his proposition, not\r\nonly because I admitted his argument, but because it would enable\r\nme to keep from the enemy a little longer our designs. Porter's\r\nfleet was on the east side of the river above the mouth of the\r\nYazoo, entirely concealed from the enemy by the dense forests that\r\nintervened. Even spies could not get near him, on account of the\r\nundergrowth and overflowed lands. Suspicions of some mysterious\r\nmovements were aroused. Our river guards discovered one day a small\r\nskiff moving quietly and mysteriously up the river near the east\r\nshore, from the direction of Vicksburg, towards the fleet. On\r\noverhauling the boat they found a small white flag, not much larger\r\nthan a handkerchief, set up in the stern, no doubt intended as a\r\nflag of truce in case of discovery. The boat, crew and passengers\r\nwere brought ashore to me. The chief personage aboard proved to be\r\nJacob Thompson, Secretary of the Interior under the administration\r\nof President Buchanan. After a pleasant conversation of half an\r\nhour or more I allowed the boat and crew, passengers and all, to\r\nreturn to Vicksburg, without creating a suspicion that there was a\r\ndoubt in my mind as to the good faith of Mr. Thompson and his\r\nflag.", 'Paragraph 370': "Admiral Porter proceeded with the preparation of the steamers\r\nfor their hazardous passage of the enemy's batteries. The great\r\nessential was to protect the boilers from the enemy's shot, and to\r\nconceal the fires under the boilers from view. This he accomplished\r\nby loading the steamers, between the guards and boilers on the\r\nboiler deck up to the deck above, with bales of hay and cotton, and\r\nthe deck in front of the boilers in the same way, adding sacks of\r\ngrain. The hay and grain would be wanted below, and could not be\r\ntransported in sufficient quantity by the muddy roads over which we\r\nexpected to march.", 'Paragraph 371': "Before this I had been collecting, from St. Louis and Chicago,\r\nyawls and barges to be used as ferries when we got below. By the\r\n16th of April Porter was ready to start on his perilous trip. The\r\nadvance, flagship Benton, Porter commanding, started at ten o'clock\r\nat night, followed at intervals of a few minutes by the Lafayette\r\nwith a captured steamer, the Price, lashed to her side, the\r\nLouisville, Mound City, Pittsburgh and Carondelet—all of\r\nthese being naval vessels. Next came the transports—Forest\r\nQueen, Silver Wave and Henry Clay, each towing barges loaded with\r\ncoal to be used as fuel by the naval and transport steamers when\r\nbelow the batteries. The gunboat Tuscumbia brought up the rear.\r\nSoon after the start a battery between Vicksburg and Warrenton\r\nopened fire across the intervening peninsula, followed by the upper\r\nbatteries, and then by batteries all along the line. The gunboats\r\nran up close under the bluffs, delivering their fire in return at\r\nshort distances, probably without much effect. They were under fire\r\nfor more than two hours and every vessel was struck many times, but\r\nwith little damage to the gunboats. The transports did not fare so\r\nwell. The Henry Clay was disabled and deserted by her crew. Soon\r\nafter a shell burst in the cotton packed about the boilers, set the\r\nvessel on fire and burned her to the water's edge. The burning\r\nmass, however, floated down to Carthage before grounding, as did\r\nalso one of the barges in tow.", 'Paragraph 372': 'The enemy were evidently expecting our fleet, for they were\r\nready to light up the river by means of bonfires on the east side\r\nand by firing houses on the point of land opposite the city on the\r\nLouisiana side. The sight was magnificent, but terrible. I\r\nwitnessed it from the deck of a river transport, run out into the\r\nmiddle of the river and as low down as it was prudent to go. My\r\nmind was much relieved when I learned that no one on the transports\r\nhad been killed and but few, if any, wounded. During the running of\r\nthe batteries men were stationed in the holds of the transports to\r\npartially stop with cotton shot-holes that might be made in the\r\nhulls. All damage was afterwards soon repaired under the direction\r\nof Admiral Porter.', 'Paragraph 373': "The experiment of passing batteries had been tried before this,\r\nhowever, during the war. Admiral Farragut had run the batteries at\r\nPort Hudson with the flagship Hartford and one iron-clad and\r\nvisited me from below Vicksburg. The 13th of February Admiral\r\nPorter had sent the gunboat Indianola, Lieutenant-Commander George\r\nBrown commanding, below. She met Colonel Ellet of the Marine\r\nbrigade below Natchez on a captured steamer. Two of the Colonel's\r\nfleet had previously run the batteries, producing the greatest\r\nconsternation among the people along the Mississippi from Vicksburg\r\nto the Red River.", 'Paragraph 374': '[Colonel Ellet reported having attacked a Confederate\r\nbattery on the Red River two days before with one of his boats, the\r\nDe Soto. Running aground, he was obliged to abandon his vessel.\r\nHowever, he reported that he set fire to her and blew her up.\r\nTwenty of his men fell into the hands of the enemy. With the\r\nbalance he escaped on the small captured steamer, the New Era, and\r\nsucceeded in passing the batteries at Grand Gulf and reaching the\r\nvicinity of Vicksburg.', 'Paragraph 375': 'The Indianola remained about the mouth of the Red River some\r\ndays, and then started up the Mississippi. The Confederates soon\r\nraised the Queen of the West, and repaired her.', 'Paragraph 376': "One of Colonel Ellet's vessels which had run the\r\nblockade on February the 2d and been sunk in the Red\r\nRiver.", 'Paragraph 377': 'With this vessel and the ram Webb, which they had had for some\r\ntime in the Red River, and two other steamers, they followed the\r\nIndianola. The latter was encumbered with barges of coal in tow,\r\nand consequently could make but little speed against the rapid\r\ncurrent of the Mississippi. The Confederate fleet overtook her just\r\nabove Grand Gulf, and attacked her after dark on the 24th of\r\nFebruary. The Indianola was superior to all the others in armament,\r\nand probably would have destroyed them or driven them away, but for\r\nher encumbrance. As it was she fought them for an hour and a half,\r\nbut, in the dark, was struck seven or eight times by the ram and\r\nother vessels, and was finally disabled and reduced to a sinking\r\ncondition. The armament was thrown overboard and the vessel run\r\nashore. Officers and crew then surrendered.', 'Paragraph 378': 'I had started McClernand with his corps of four divisions on the\r\n29th of March, by way of Richmond, Louisiana, to New Carthage,\r\nhoping that he might capture Grand Gulf before the balance of the\r\ntroops could get there; but the roads were very bad, scarcely above\r\nwater yet. Some miles from New Carthage the levee to Bayou Vidal\r\nwas broken in several places, overflowing the roads for the\r\ndistance of two miles. Boats were collected from the surrounding\r\nbayous, and some constructed on the spot from such material as\r\ncould be collected, to transport the troops across the overflowed\r\ninterval. By the 6th of April McClernand had reached New Carthage\r\nwith one division and its artillery, the latter ferried through the\r\nwoods by these boats. On the 17th I visited New Carthage in person,\r\nand saw that the process of getting troops through in the way we\r\nwere doing was so tedious that a better method must be devised. The\r\nwater was falling, and in a few days there would not be depth\r\nenough to use boats; nor would the land be dry enough to march\r\nover. McClernand had already found a new route from Smith\'s\r\nplantation where the crevasse occurred, to Perkins\' plantation,\r\neight to twelve miles below New Carthage. This increased the march\r\nfrom Milliken\'s Bend from twenty-seven to nearly forty miles. Four\r\nbridges had to be built across bayous, two of them each over six\r\nhundred feet long, making about two thousand feet of bridging in\r\nall. The river falling made the current in these bayous very rapid,\r\nincreasing the difficulty of building and permanently fastening\r\nthese bridges; but the ingenuity of the "Yankee soldier" was equal\r\nto any emergency. The bridges were soon built of such material as\r\ncould be found near by, and so substantial were they that not a\r\nsingle mishap occurred in crossing all the army with artillery,\r\ncavalry and wagon trains, except the loss of one siege gun (a\r\nthirty-two pounder). This, if my memory serves me correctly, broke\r\nthrough the only pontoon bridge we had in all our march across the\r\npeninsula. These bridges were all built by McClernand\'s command,\r\nunder the supervision of Lieutenant Hains of the Engineer\r\nCorps.', 'Paragraph 379': "I returned to Milliken's Bend on the 18th or 19th, and on the\r\n20th issued the following final order for the movement of\r\ntroops:", 'Paragraph 380': "HEADQUARTERS DEPARTMENT OF THE TENNESSEE,\r\nMILLIKEN'S BEND, LOUISIANA,\r\nApril 20, 1863.", 'Paragraph 381': 'Special Orders, No. 110.\n\r\n*****************************************', 'Paragraph 382': 'VIII. The following orders are published for the information and\r\nguidance of the "Army in the Field," in its present movement to\r\nobtain a foothold on the east bank of the Mississippi River, from\r\nwhich Vicksburg can be approached by practicable roads.', 'Paragraph 383': 'First.—The Thirteenth army corps, Major-General John A.\r\nMcClernand commanding, will constitute the right wing.', 'Paragraph 384': 'Second.—The Fifteenth army corps, Major-General W. T.\r\nSherman commanding, will constitute the left wing.', 'Paragraph 385': 'Third.—The Seventeenth army corps, Major-General James B.\r\nMcPherson commanding, will constitute the centre.', 'Paragraph 386': 'Fourth.—The order of march to New Carthage will be from\r\nright to left.', 'Paragraph 387': 'Fifth.—Reserves will be formed by divisions from each army\r\ncorps; or, an entire army corps will be held as a reserve, as\r\nnecessity may require. When the reserve is formed by divisions,\r\neach division will remain under the immediate command of its\r\nrespective corps commander, unless otherwise specially ordered for\r\na particular emergency.', 'Paragraph 388': 'Sixth.—Troops will be required to bivouac, until proper\r\nfacilities can be afforded for the transportation of camp\r\nequipage.', 'Paragraph 389': 'Seventh.—In the present movement, one tent will be allowed\r\nto each company for the protection of rations from rain; one wall\r\ntent for each regimental headquarters; one wall tent for each\r\nbrigade headquarters; and one wall tent for each division\r\nheadquarters; corps commanders having the books and blanks of their\r\nrespective commands to provide for, are authorized to take such\r\ntents as are absolutely necessary, but not to exceed the number\r\nallowed by General Orders No. 160, A. G. O., series of 1862.', 'Paragraph 390': 'Eighth.—All the teams of the three army corps, under the\r\nimmediate charge of the quartermasters bearing them on their\r\nreturns, will constitute a train for carrying supplies and ordnance\r\nand the authorized camp equipage of the army.', 'Paragraph 391': 'Ninth.—As fast as the Thirteenth army corps advances, the\r\nSeventeenth army corps will take its place; and it, in turn, will\r\nbe followed in like manner by the Fifteenth army corps.', 'Paragraph 392': 'Tenth.—Two regiments from each army corps will be detailed\r\nby corps commanders, to guard the lines from Richmond to New\r\nCarthage.', 'Paragraph 393': "Eleventh.—General hospitals will be established by the\r\nmedical director between Duckport and Milliken's Bend. All sick and\r\ndisabled soldiers will be left in these hospitals. Surgeons in\r\ncharge of hospitals will report convalescents as fast as they\r\nbecome fit for duty. Each corps commander will detail an\r\nintelligent and good drill officer, to remain behind and take\r\ncharge of the convalescents of their respective corps; officers so\r\ndetailed will organize the men under their charge into squads and\r\ncompanies, without regard to the regiments they belong to; and in\r\nthe absence of convalescent commissioned officers to command them,\r\nwill appoint non-commissioned officers or privates. The force so\r\norganized will constitute the guard of the line from Duckport to\r\nMilliken's Bend. They will furnish all the guards and details\r\nrequired for general hospitals, and with the contrabands that may\r\nbe about the camps, will furnish all the details for loading and\r\nunloading boats.", 'Paragraph 394': "Twelfth.—The movement of troops from Milliken's Bend to\r\nNew Carthage will be so conducted as to allow the transportation of\r\nten days' supply of rations, and one-half the allowance of\r\nordnance, required by previous orders.", 'Paragraph 395': 'Thirteenth.—Commanders are authorized and enjoined to\r\ncollect all the beef cattle, corn and other necessary supplies on\r\nthe line of march; but wanton destruction of property, taking of\r\narticles useless for military purposes, insulting citizens, going\r\ninto and searching houses without proper orders from division\r\ncommanders, are positively prohibited. All such irregularities must\r\nbe summarily punished.', 'Paragraph 396': "Fourteenth.—Brigadier-General J. C. Sullivan is appointed\r\nto the command of all the forces detailed for the protection of the\r\nline from here to New Carthage. His particular attention is called\r\nto General Orders, No. 69, from Adjutant-General's Office,\r\nWashington, of date March 20, 1863.", 'Paragraph 397': 'By order of\r\nMAJOR-GENERAL U. S. GRANT.', 'Paragraph 398': "McClernand was already below on the Mississippi. Two of\r\nMcPherson's divisions were put upon the march immediately. The\r\nthird had not yet arrived from Lake Providence; it was on its way\r\nto Milliken's Bend and was to follow on arrival.", 'Paragraph 399': "Sherman was to follow McPherson. Two of his divisions were at\r\nDuckport and Young's Point, and the third under Steele was under\r\norders to return from Greenville, Mississippi, where it had been\r\nsent to expel a rebel battery that had been annoying our\r\ntransports.", 'Paragraph 400': "It had now become evident that the army could not be rationed by\r\na wagon train over the single narrow and almost impassable road\r\nbetween Milliken's Bend and Perkins' plantation. Accordingly six\r\nmore steamers were protected as before, to run the batteries, and\r\nwere loaded with supplies. They took twelve barges in tow, loaded\r\nalso with rations. On the night of the 22d of April they ran the\r\nbatteries, five getting through more or less disabled while one was\r\nsunk. About half the barges got through with their needed\r\nfreight.", 'Paragraph 401': "When it was first proposed to run the blockade at Vicksburg with\r\nriver steamers there were but two captains or masters who were\r\nwilling to accompany their vessels, and but one crew. Volunteers\r\nwere called for from the army, men who had had experience in any\r\ncapacity in navigating the western rivers. Captains, pilots, mates,\r\nengineers and deck-hands enough presented themselves to take five\r\ntimes the number of vessels we were moving through this dangerous\r\nordeal. Most of them were from Logan's division, composed generally\r\nof men from the southern part of Illinois and from Missouri. All\r\nbut two of the steamers were commanded by volunteers from the army,\r\nand all but one so manned. In this instance, as in all others\r\nduring the war, I found that volunteers could be found in the ranks\r\nand among the commissioned officers to meet every call for aid\r\nwhether mechanical or professional. Colonel W. S. Oliver was master\r\nof transportation on this occasion by special detail.", 'Paragraph 402': "On the 24th my headquarters were with the advance at Perkins'\r\nplantation. Reconnoissances were made in boats to ascertain whether\r\nthere was high land on the east shore of the river where we might\r\nland above Grand Gulf. There was none practicable. Accordingly the\r\ntroops were set in motion for Hard Times, twenty-two miles farther\r\ndown the river and nearly opposite Grand Gulf. The loss of two\r\nsteamers and six barges reduced our transportation so that only\r\n10,000 men could be moved by water. Some of the steamers that had\r\ngot below were injured in their machinery, so that they were only\r\nuseful as barges towed by those less severely injured. All the\r\ntroops, therefore, except what could be transported in one trip,\r\nhad to march. The road lay west of Lake St. Joseph. Three large\r\nbayous had to be crossed. They were rapidly bridged in the same\r\nmanner as those previously encountered.", 'Paragraph 403': '[NOTE.—On this occasion Governor Richard Yates,\r\nof Illinois, happened to be on a visit to the army and accompanied\r\nme to Carthage. I furnished an ambulance for his use and that of\r\nsome of the State officers who accompanied him.]', 'Paragraph 404': "On the 27th McClernand's corps was all at Hard Times, and\r\nMcPherson's was following closely. I had determined to make the\r\nattempt to effect a landing on the east side of the river as soon\r\nas possible. Accordingly, on the morning of the 29th, McClernand\r\nwas directed to embark all the troops from his corps that our\r\ntransports and barges could carry. About 10,000 men were so\r\nembarked. The plan was to have the navy silence the guns at Grand\r\nGulf, and to have as many men as possible ready to debark in the\r\nshortest possible time under cover of the fire of the navy and\r\ncarry the works by storm. The following order was issued:", 'Paragraph 405': 'PERKINS PLANTATION, LA.,\r\nApril 27,1863.', 'Paragraph 406': 'MAJOR-GENERAL J. A. MCCLERNAND,\r\nCommanding 13th A. C.', 'Paragraph 407': "Commence immediately the embarkation of your corps, or so much\r\nof it as there is transportation for. Have put aboard the artillery\r\nand every article authorized in orders limiting baggage, except the\r\nmen, and hold them in readiness, with their places assigned, to be\r\nmoved at a moment's warning.", 'Paragraph 408': 'All the troops you may have, except those ordered to remain\r\nbehind, send to a point nearly opposite Grand Gulf, where you see,\r\nby special orders of this date, General McPherson is ordered to\r\nsend one division.', 'Paragraph 409': 'The plan of the attack will be for the navy to attack and\r\nsilence all the batteries commanding the river. Your corps will be\r\non the river, ready to run to and debark on the nearest eligible\r\nland below the promontory first brought to view passing down the\r\nriver. Once on shore, have each commander instructed beforehand to\r\nform his men the best the ground will admit of, and take possession\r\nof the most commanding points, but avoid separating your command so\r\nthat it cannot support itself. The first object is to get a\r\nfoothold where our troops can maintain themselves until such time\r\nas preparations can be made and troops collected for a forward\r\nmovement.', 'Paragraph 410': 'Admiral Porter has proposed to place his boats in the position\r\nindicated to you a few days ago, and to bring over with them such\r\ntroops as may be below the city after the guns of the enemy are\r\nsilenced.', 'Paragraph 411': 'It may be that the enemy will occupy positions back from the\r\ncity, out of range of the gunboats, so as to make it desirable to\r\nrun past Grand Gulf and land at Rodney. In case this should prove\r\nthe plan, a signal will be arranged and you duly informed, when the\r\ntransports are to start with this view. Or, it may be expedient for\r\nthe boats to run past, but not the men. In this case, then, the\r\ntransports would have to be brought back to where the men could\r\nland and move by forced marches to below Grand Gulf, re-embark\r\nrapidly and proceed to the latter place. There will be required,\r\nthen, three signals; one, to indicate that the transports can run\r\ndown and debark the troops at Grand Gulf; one, that the transports\r\ncan run by without the troops; and the last, that the transports\r\ncan run by with the troops on board.', 'Paragraph 412': 'Should the men have to march, all baggage and artillery will be\r\nleft to run the blockade.', 'Paragraph 413': "If not already directed, require your men to keep three days'\r\nrations in their haversacks, not to be touched until a movement\r\ncommences.", 'Paragraph 414': 'U. S. GRANT,\r\nMajor-General.', 'Paragraph 415': "At 8 o'clock A.M., 29th, Porter made the attack with his entire\r\nstrength present, eight gunboats. For nearly five and a half hours\r\nthe attack was kept up without silencing a single gun of the enemy.\r\nAll this time McClernand's 10,000 men were huddled together on the\r\ntransports in the stream ready to attempt a landing if signalled. I\r\noccupied a tug from which I could see the effect of the battle on\r\nboth sides, within range of the enemy's guns; but a small tug,\r\nwithout armament, was not calculated to attract the fire of\r\nbatteries while they were being assailed themselves. About\r\nhalf-past one the fleet withdrew, seeing their efforts were\r\nentirely unavailing. The enemy ceased firing as soon as we\r\nwithdrew. I immediately signalled the Admiral and went aboard his\r\nship. The navy lost in this engagement eighteen killed and\r\nfifty-six wounded. A large proportion of these were of the crew of\r\nthe flagship, and most of those from a single shell which\r\npenetrated the ship's side and exploded between decks where the men\r\nwere working their guns. The sight of the mangled and dying men\r\nwhich met my eye as I boarded the ship was sickening.", 'Paragraph 416': 'Grand Gulf is on a high bluff where the river runs at the very\r\nfoot of it. It is as defensible upon its front as Vicksburg and, at\r\nthat time, would have been just as impossible to capture by a front\r\nattack. I therefore requested Porter to run the batteries with his\r\nfleet that night, and to take charge of the transports, all of\r\nwhich would be wanted below.', 'Paragraph 417': 'There is a long tongue of land from the Louisiana side extending\r\ntowards Grand Gulf, made by the river running nearly east from\r\nabout three miles above and nearly in the opposite direction from\r\nthat point for about the same distance below. The land was so low\r\nand wet that it would not have been practicable to march an army\r\nacross but for a levee. I had had this explored before, as well as\r\nthe east bank below to ascertain if there was a possible point of\r\ndebarkation north of Rodney. It was found that the top of the levee\r\nafforded a good road to march upon.', 'Paragraph 418': 'Porter, as was always the case with him, not only acquiesced in\r\nthe plan, but volunteered to use his entire fleet as transports. I\r\nhad intended to make this request, but he anticipated me. At dusk,\r\nwhen concealed from the view of the enemy at Grand Gulf, McClernand\r\nlanded his command on the west bank. The navy and transports ran\r\nthe batteries successfully. The troops marched across the point of\r\nland under cover of night, unobserved. By the time it was light the\r\nenemy saw our whole fleet, ironclads, gunboats, river steamers and\r\nbarges, quietly moving down the river three miles below them,\r\nblack, or rather blue, with National troops.', 'Paragraph 419': 'When the troops debarked, the evening of the 29th, it was\r\nexpected that we would have to go to Rodney, about nine miles\r\nbelow, to find a landing; but that night a colored man came in who\r\ninformed me that a good landing would be found at Bruinsburg, a few\r\nmiles above Rodney, from which point there was a good road leading\r\nto Port Gibson some twelve miles in the interior. The information\r\nwas found correct, and our landing was effected without\r\nopposition.', 'Paragraph 420': "Sherman had not left his position above Vicksburg yet. On the\r\nmorning of the 27th I ordered him to create a diversion by moving\r\nhis corps up the Yazoo and threatening an attack on Haines'\r\nBluff.", 'Paragraph 421': 'My object was to compel Pemberton to keep as much force about\r\nVicksburg as I could, until I could secure a good footing on high\r\nland east of the river. The move was eminently successful and, as\r\nwe afterwards learned, created great confusion about Vicksburg and\r\ndoubts about our real design. Sherman moved the day of our attack\r\non Grand Gulf, the 29th, with ten regiments of his command and\r\neight gunboats which Porter had left above Vicksburg.', 'Paragraph 422': "He debarked his troops and apparently made every preparation to\r\nattack the enemy while the navy bombarded the main forts at Haines'\r\nBluff. This move was made without a single casualty in either\r\nbranch of the service. On the first of May Sherman received orders\r\nfrom me (sent from Hard Times the evening of the 29th of April) to\r\nwithdraw from the front of Haines' Bluff and follow McPherson with\r\ntwo divisions as fast as he could.", 'Paragraph 423': "I had established a depot of supplies at Perkins' plantation.\r\nNow that all our gunboats were below Grand Gulf it was possible\r\nthat the enemy might fit out boats in the Big Black with improvised\r\narmament and attempt to destroy these supplies. McPherson was at\r\nHard Times with a portion of his corps, and the depot was protected\r\nby a part of his command. The night of the 29th I directed him to\r\narm one of the transports with artillery and send it up to Perkins'\r\nplantation as a guard; and also to have the siege guns we had\r\nbrought along moved there and put in position.", 'Paragraph 424': "The embarkation below Grand Gulf took place at De Shroon's,\r\nLouisiana, six miles above Bruinsburg, Mississippi. Early on the\r\nmorning of 30th of April McClernand's corps and one division of\r\nMcPherson's corps were speedily landed.", 'Paragraph 425': "When this was effected I felt a degree of relief scarcely ever\r\nequalled since. Vicksburg was not yet taken it is true, nor were\r\nits defenders demoralized by any of our previous moves. I was now\r\nin the enemy's country, with a vast river and the stronghold of\r\nVicksburg between me and my base of supplies. But I was on dry\r\nground on the same side of the river with the enemy. All the\r\ncampaigns, labors, hardships and exposures from the month of\r\nDecember previous to this time that had been made and endured, were\r\nfor the accomplishment of this one object.", 'Paragraph 426': "I had with me the 13th corps, General McClernand commanding, and\r\ntwo brigades of Logan's division of the 17th corps, General\r\nMcPherson commanding—in all not more than twenty thousand men\r\nto commence the campaign with. These were soon reinforced by the\r\nremaining brigade of Logan's division and Crocker's division of the\r\n17th corps. On the 7th of May I was further reinforced by Sherman\r\nwith two divisions of his, the 15th corps. My total force was then\r\nabout thirty-three thousand men.", 'Paragraph 427': "The enemy occupied Grand Gulf, Haines' Bluff and Jackson with a\r\nforce of nearly sixty thousand men. Jackson is fifty miles east of\r\nVicksburg and is connected with it by a railroad. My first problem\r\nwas to capture Grand Gulf to use as a base.", 'Paragraph 428': "Bruinsburg is two miles from high ground. The bottom at that\r\npoint is higher than most of the low land in the valley of the\r\nMississippi, and a good road leads to the bluff. It was natural to\r\nexpect the garrison from Grand Gulf to come out to meet us and\r\nprevent, if they could, our reaching this solid base. Bayou Pierre\r\nenters the Mississippi just above Bruinsburg and, as it is a\r\nnavigable stream and was high at the time, in order to intercept us\r\nthey had to go by Port Gibson, the nearest point where there was a\r\nbridge to cross upon. This more than doubled the distance from\r\nGrand Gulf to the high land back of Bruinsburg. No time was to be\r\nlost in securing this foothold. Our transportation was not\r\nsufficient to move all the army across the river at one trip, or\r\neven two; but the landing of the 13th corps and one division of the\r\n17th was effected during the day, April 30th, and early evening.\r\nMcClernand was advanced as soon as ammunition and two days' rations\r\n(to last five) could be issued to his men. The bluffs were reached\r\nan hour before sunset and McClernand was pushed on, hoping to reach\r\nPort Gibson and save the bridge spanning the Bayou Pierre before\r\nthe enemy could get there; for crossing a stream in the presence of\r\nan enemy is always difficult. Port Gibson, too, is the starting\r\npoint of roads to Grand Gulf, Vicksburg and Jackson.", 'Paragraph 429': "McClernand's advance met the enemy about five miles west of Port\r\nGibson at Thompson's plantation. There was some firing during the\r\nnight, but nothing rising to the dignity of a battle until\r\ndaylight. The enemy had taken a strong natural position with most\r\nof the Grand Gulf garrison, numbering about seven or eight thousand\r\nmen, under General Bowen. His hope was to hold me in check until\r\nreinforcements under Loring could reach him from Vicksburg; but\r\nLoring did not come in time to render much assistance south of Port\r\nGibson. Two brigades of McPherson's corps followed McClernand as\r\nfast as rations and ammunition could be issued, and were ready to\r\ntake position upon the battlefield whenever the 13th corps could be\r\ngot out of the way.", 'Paragraph 430': 'The country in this part of Mississippi stands on edge, as it\r\nwere, the roads running along the ridges except when they\r\noccasionally pass from one ridge to another. Where there are no\r\nclearings the sides of the hills are covered with a very heavy\r\ngrowth of timber and with undergrowth, and the ravines are filled\r\nwith vines and canebrakes, almost impenetrable. This makes it easy\r\nfor an inferior force to delay, if not defeat, a far superior\r\none.', 'Paragraph 431': "Near the point selected by Bowen to defend, the road to Port\r\nGibson divides, taking two ridges which do not diverge more than a\r\nmile or two at the widest point. These roads unite just outside the\r\ntown. This made it necessary for McClernand to divide his force. It\r\nwas not only divided, but it was separated by a deep ravine of the\r\ncharacter above described. One flank could not reinforce the other\r\nexcept by marching back to the junction of the roads. McClernand\r\nput the divisions of Hovey, Carr and A. J. Smith upon the\r\nright-hand branch and Osterhaus on the left. I was on the field by\r\nten A.M., and inspected both flanks in person. On the right the\r\nenemy, if not being pressed back, was at least not repulsing our\r\nadvance. On the left, however, Osterhaus was not faring so well. He\r\nhad been repulsed with some loss. As soon as the road could be\r\ncleared of McClernand's troops I ordered up McPherson, who was\r\nclose upon the rear of the 13th corps, with two brigades of Logan's\r\ndivision. This was about noon. I ordered him to send one brigade\r\n(General John E. Smith's was selected) to support Osterhaus, and to\r\nmove to the left and flank the enemy out of his position. This\r\nmovement carried the brigade over a deep ravine to a third ridge\r\nand, when Smith's troops were seen well through the ravine,\r\nOsterhaus was directed to renew his front attack. It was successful\r\nand unattended by heavy loss. The enemy was sent in full retreat on\r\ntheir right, and their left followed before sunset. While the\r\nmovement to our left was going on, McClernand, who was with his\r\nright flank, sent me frequent requests for reinforcements, although\r\nthe force with him was not being pressed. I had been upon the\r\nground and knew it did not admit of his engaging all the men he\r\nhad. We followed up our victory until night overtook us about two\r\nmiles from Port Gibson; then the troops went into bivouac for the\r\nnight.", 'Paragraph 432': "We started next morning for Port Gibson as soon as it was light\r\nenough to see the road. We were soon in the town, and I was\r\ndelighted to find that the enemy had not stopped to contest our\r\ncrossing further at the bridge, which he had burned. The troops\r\nwere set to work at once to construct a bridge across the South\r\nFork of the Bayou Pierre. At this time the water was high and the\r\ncurrent rapid. What might be called a raft-bridge was soon\r\nconstructed from material obtained from wooden buildings, stables,\r\nfences, etc., which sufficed for carrying the whole army over\r\nsafely. Colonel J. H. Wilson, a member of my staff, planned and\r\nsuperintended the construction of this bridge, going into the water\r\nand working as hard as any one engaged. Officers and men generally\r\njoined in this work. When it was finished the army crossed and\r\nmarched eight miles beyond to the North Fork that day. One brigade\r\nof Logan's division was sent down the stream to occupy the\r\nattention of a rebel battery, which had been left behind with\r\ninfantry supports to prevent our repairing the burnt railroad\r\nbridge. Two of his brigades were sent up the bayou to find a\r\ncrossing and reach the North Fork to repair the bridge there. The\r\nenemy soon left when he found we were building a bridge elsewhere.\r\nBefore leaving Port Gibson we were reinforced by Crocker's\r\ndivision, McPherson's corps, which had crossed the Mississippi at\r\nBruinsburg and come up without stopping except to get two days'\r\nrations. McPherson still had one division west of the Mississippi\r\nRiver, guarding the road from Milliken's Bend to the river below\r\nuntil Sherman's command should relieve it.", 'Paragraph 433': "On leaving Bruinsburg for the front I left my son Frederick, who\r\nhad joined me a few weeks before, on board one of the gunboats\r\nasleep, and hoped to get away without him until after Grand Gulf\r\nshould fall into our hands; but on waking up he learned that I had\r\ngone, and being guided by the sound of the battle raging at\r\nThompson's Hill—called the Battle of Port Gibson—found\r\nhis way to where I was. He had no horse to ride at the time, and I\r\nhad no facilities for even preparing a meal. He, therefore, foraged\r\naround the best he could until we reached Grand Gulf. Mr. C. A.\r\nDana, then an officer of the War Department, accompanied me on the\r\nVicksburg campaign and through a portion of the siege. He was in\r\nthe same situation as Fred so far as transportation and mess\r\narrangements were concerned. The first time I call to mind seeing\r\neither of them, after the battle, they were mounted on two enormous\r\nhorses, grown white from age, each equipped with dilapidated\r\nsaddles and bridles.", 'Paragraph 434': 'Our trains arrived a few days later, after which we were all\r\nperfectly equipped.', 'Paragraph 435': 'My son accompanied me throughout the campaign and siege, and\r\ncaused no anxiety either to me or to his mother, who was at home.\r\nHe looked out for himself and was in every battle of the campaign.\r\nHis age, then not quite thirteen, enabled him to take in all he\r\nsaw, and to retain a recollection of it that would not be possible\r\nin more mature years.', 'Paragraph 436': "When the movement from Bruinsburg commenced we were without a\r\nwagon train. The train still west of the Mississippi was carried\r\naround with proper escort, by a circuitous route from Milliken's\r\nBend to Hard Times seventy or more miles below, and did not get up\r\nfor some days after the battle of Port Gibson. My own horses,\r\nheadquarters' transportation, servants, mess chest, and everything\r\nexcept what I had on, was with this train. General A. J. Smith\r\nhappened to have an extra horse at Bruinsburg which I borrowed,\r\nwith a saddle-tree without upholstering further than stirrups. I\r\nhad no other for nearly a week.", 'Paragraph 437': 'It was necessary to have transportation for ammunition.\r\nProvisions could be taken from the country; but all the ammunition\r\nthat can be carried on the person is soon exhausted when there is\r\nmuch fighting. I directed, therefore, immediately on landing that\r\nall the vehicles and draft animals, whether horses, mules, or oxen,\r\nin the vicinity should be collected and loaded to their capacity\r\nwith ammunition. Quite a train was collected during the 30th, and a\r\nmotley train it was. In it could be found fine carriages, loaded\r\nnearly to the top with boxes of cartridges that had been pitched in\r\npromiscuously, drawn by mules with plough, harness, straw collars,\r\nrope-lines, etc.; long-coupled wagons, with racks for carrying\r\ncotton bales, drawn by oxen, and everything that could be found in\r\nthe way of transportation on a plantation, either for use or\r\npleasure. The making out of provision returns was stopped for the\r\ntime. No formalities were to retard our progress until a position\r\nwas secured when the time could be spared to observe them.', 'Paragraph 438': 'It was at Port Gibson I first heard through a Southern paper of\r\nthe complete success of Colonel Grierson, who was making a raid\r\nthrough central Mississippi. He had started from La Grange April\r\n17th with three regiments of about 1,700 men. On the 21st he had\r\ndetached Colonel Hatch with one regiment to destroy the railroad\r\nbetween Columbus and Macon and then return to La Grange. Hatch had\r\na sharp fight with the enemy at Columbus and retreated along the\r\nrailroad, destroying it at Okalona and Tupelo, and arriving in La\r\nGrange April 26. Grierson continued his movement with about 1,000\r\nmen, breaking the Vicksburg and Meridian railroad and the New\r\nOrleans and Jackson railroad, arriving at Baton Rouge May 2d. This\r\nraid was of great importance, for Grierson had attracted the\r\nattention of the enemy from the main movement against\r\nVicksburg.', 'Paragraph 439': "During the night of the 2d of May the bridge over the North Fork\r\nwas repaired, and the troops commenced crossing at five the next\r\nmorning. Before the leading brigade was over it was fired upon by\r\nthe enemy from a commanding position; but they were soon driven\r\noff. It was evident that the enemy was covering a retreat from\r\nGrand Gulf to Vicksburg. Every commanding position from this\r\n(Grindstone) crossing to Hankinson's ferry over the Big Black was\r\noccupied by the retreating foe to delay our progress. McPherson,\r\nhowever, reached Hankinson's ferry before night, seized the ferry\r\nboat, and sent a detachment of his command across and several miles\r\nnorth on the road to Vicksburg. When the junction of the road going\r\nto Vicksburg with the road from Grand Gulf to Raymond and Jackson\r\nwas reached, Logan with his division was turned to the left towards\r\nGrand Gulf. I went with him a short distance from this junction.\r\nMcPherson had encountered the largest force yet met since the\r\nbattle of Port Gibson and had a skirmish nearly approaching a\r\nbattle; but the road Logan had taken enabled him to come up on the\r\nenemy's right flank, and they soon gave way. McPherson was ordered\r\nto hold Hankinson's ferry and the road back to Willow Springs with\r\none division; McClernand, who was now in the rear, was to join in\r\nthis as well as to guard the line back down the bayou. I did not\r\nwant to take the chances of having an enemy lurking in our\r\nrear.", 'Paragraph 440': 'On the way from the junction to Grand Gulf, where the road comes\r\ninto the one from Vicksburg to the same place six or seven miles\r\nout, I learned that the last of the enemy had retreated past that\r\nplace on their way to Vicksburg. I left Logan to make the proper\r\ndisposition of his troops for the night, while I rode into the town\r\nwith an escort of about twenty cavalry. Admiral Porter had already\r\narrived with his fleet. The enemy had abandoned his heavy guns and\r\nevacuated the place.', 'Paragraph 441': "When I reached Grand Gulf May 3d I had not been with my baggage\r\nsince the 27th of April and consequently had had no change of\r\nunderclothing, no meal except such as I could pick up sometimes at\r\nother headquarters, and no tent to cover me. The first thing I did\r\nwas to get a bath, borrow some fresh underclothing from one of the\r\nnaval officers and get a good meal on the flag-ship. Then I wrote\r\nletters to the general-in-chief informing him of our present\r\nposition, dispatches to be telegraphed from Cairo, orders to\r\nGeneral Sullivan commanding above Vicksburg, and gave orders to all\r\nmy corps commanders. About twelve o'clock at night I was through my\r\nwork and started for Hankinson's ferry, arriving there before\r\ndaylight. While at Grand Gulf I heard from Banks, who was on the\r\nRed River, and who said that he could not be at Port Hudson before\r\nthe 10th of May and then with only 15,000 men. Up to this time my\r\nintention had been to secure Grand Gulf, as a base of supplies,\r\ndetach McClernand's corps to Banks and co-operate with him in the\r\nreduction of Port Hudson.", 'Paragraph 442': 'The news from Banks forced upon me a different plan of campaign\r\nfrom the one intended. To wait for his co-operation would have\r\ndetained me at least a month. The reinforcements would not have\r\nreached ten thousand men after deducting casualties and necessary\r\nriver guards at all high points close to the river for over three\r\nhundred miles. The enemy would have strengthened his position and\r\nbeen reinforced by more men than Banks could have brought. I\r\ntherefore determined to move independently of Banks, cut loose from\r\nmy base, destroy the rebel force in rear of Vicksburg and invest or\r\ncapture the city.', 'Paragraph 443': 'Grand Gulf was accordingly given up as a base and the\r\nauthorities at Washington were notified. I knew well that Halleck\'s\r\ncaution would lead him to disapprove of this course; but it was the\r\nonly one that gave any chance of success. The time it would take to\r\ncommunicate with Washington and get a reply would be so great that\r\nI could not be interfered with until it was demonstrated whether my\r\nplan was practicable. Even Sherman, who afterwards ignored bases of\r\nsupplies other than what were afforded by the country while\r\nmarching through four States of the Confederacy with an army more\r\nthan twice as large as mine at this time, wrote me from Hankinson\'s\r\nferry, advising me of the impossibility of supplying our army over\r\na single road. He urged me to "stop all troops till your army is\r\npartially supplied with wagons, and then act as quick as possible;\r\nfor this road will be jammed, as sure as life." To this I replied:\r\n"I do not calculate upon the possibility of supplying the army with\r\nfull rations from Grand Gulf. I know it will be impossible without\r\nconstructing additional roads. What I do expect is to get up what\r\nrations of hard bread, coffee and salt we can, and make the country\r\nfurnish the balance." We started from Bruinsburg with an average of\r\nabout two days\' rations, and received no more from our own supplies\r\nfor some days; abundance was found in the mean time. A delay would\r\ngive the enemy time to reinforce and fortify.', 'Paragraph 444': "McClernand's and McPherson's commands were kept substantially as\r\nthey were on the night of the 2d, awaiting supplies sufficient to\r\ngive them three days' rations in haversacks. Beef, mutton, poultry\r\nand forage were found in abundance. Quite a quantity of bacon and\r\nmolasses was also secured from the country, but bread and coffee\r\ncould not be obtained in quantity sufficient for all the men. Every\r\nplantation, however, had a run of stone, propelled by mule power,\r\nto grind corn for the owners and their slaves. All these were kept\r\nrunning while we were stopping, day and night, and when we were\r\nmarching, during the night, at all plantations covered by the\r\ntroops. But the product was taken by the troops nearest by, so that\r\nthe majority of the command was destined to go without bread until\r\na new base was established on the Yazoo above Vicksburg.", 'Paragraph 445': 'While the troops were awaiting the arrival of rations I ordered\r\nreconnoissances made by McClernand and McPherson, with the view of\r\nleading the enemy to believe that we intended to cross the Big\r\nBlack and attack the city at once.', 'Paragraph 446': "On the 6th Sherman arrived at Grand Gulf and crossed his command\r\nthat night and the next day. Three days' rations had been brought\r\nup from Grand Gulf for the advanced troops and were issued. Orders\r\nwere given for a forward movement the next day. Sherman was\r\ndirected to order up Blair, who had been left behind to guard the\r\nroad from Milliken's Bend to Hard Times with two brigades.", 'Paragraph 447': "The quartermaster at Young's Point was ordered to send two\r\nhundred wagons with Blair, and the commissary was to load them with\r\nhard bread, coffee, sugar, salt and one hundred thousand pounds of\r\nsalt meat.", 'Paragraph 448': "On the 3d Hurlbut, who had been left at Memphis, was ordered to\r\nsend four regiments from his command to Milliken's Bend to relieve\r\nBlair's division, and on the 5th he was ordered to send Lauman's\r\ndivision in addition, the latter to join the army in the field. The\r\nfour regiments were to be taken from troops near the river so that\r\nthere would be no delay.", 'Paragraph 449': "During the night of the 6th McPherson drew in his troops north\r\nof the Big Black and was off at an early hour on the road to\r\nJackson, via Rocky Springs, Utica and Raymond. That night he and\r\nMcClernand were both at Rocky Springs ten miles from Hankinson's\r\nferry. McPherson remained there during the 8th, while McClernand\r\nmoved to Big Sandy and Sherman marched from Grand Gulf to\r\nHankinson's ferry. The 9th, McPherson moved to a point within a few\r\nmiles west of Utica; McClernand and Sherman remained where they\r\nwere. On the 10th McPherson moved to Utica, Sherman to Big Sandy;\r\nMcClernand was still at Big Sandy. The 11th, McClernand was at Five\r\nMile Creek; Sherman at Auburn; McPherson five miles advanced from\r\nUtica. May 12th, McClernand was at Fourteen Mile Creek; Sherman at\r\nFourteen Mile Creek; McPherson at Raymond after a battle.", 'Paragraph 450': "After McPherson crossed the Big Black at Hankinson's ferry\r\nVicksburg could have been approached and besieged by the south\r\nside. It is not probable, however, that Pemberton would have\r\npermitted a close besiegement. The broken nature of the ground\r\nwould have enabled him to hold a strong defensible line from the\r\nriver south of the city to the Big Black, retaining possession of\r\nthe railroad back to that point. It was my plan, therefore, to get\r\nto the railroad east of Vicksburg, and approach from that\r\ndirection. Accordingly, McPherson's troops that had crossed the Big\r\nBlack were withdrawn and the movement east to Jackson\r\ncommenced.", 'Paragraph 451': "As has been stated before, the country is very much broken and\r\nthe roads generally confined to the tops of the hills. The troops\r\nwere moved one (sometimes two) corps at a time to reach designated\r\npoints out parallel to the railroad and only from six to ten miles\r\nfrom it. McClernand's corps was kept with its left flank on the Big\r\nBlack guarding all the crossings. Fourteen Mile Creek, a stream\r\nsubstantially parallel with the railroad, was reached and crossings\r\neffected by McClernand and Sherman with slight loss. McPherson was\r\nto the right of Sherman, extending to Raymond. The cavalry was used\r\nin this advance in reconnoitring to find the roads: to cover our\r\nadvances and to find the most practicable routes from one command\r\nto another so they could support each other in case of an attack.\r\nIn making this move I estimated Pemberton's movable force at\r\nVicksburg at about eighteen thousand men, with smaller forces at\r\nHaines' Bluff and Jackson. It would not be possible for Pemberton\r\nto attack me with all his troops at one place, and I determined to\r\nthrow my army between his and fight him in detail. This was done\r\nwith success, but I found afterwards that I had entirely\r\nunder-estimated Pemberton's strength.", 'Paragraph 452': "Up to this point our movements had been made without serious\r\nopposition. My line was now nearly parallel with the Jackson and\r\nVicksburg railroad and about seven miles south of it. The right was\r\nat Raymond eighteen miles from Jackson, McPherson commanding;\r\nSherman in the centre on Fourteen Mile Creek, his advance thrown\r\nacross; McClernand to the left, also on Fourteen Mile Creek,\r\nadvance across, and his pickets within two miles of Edward's\r\nstation, where the enemy had concentrated a considerable force and\r\nwhere they undoubtedly expected us to attack. McClernand's left was\r\non the Big Black. In all our moves, up to this time, the left had\r\nhugged the Big Black closely, and all the ferries had been guarded\r\nto prevent the enemy throwing a force on our rear.", 'Paragraph 453': "McPherson encountered the enemy, five thousand strong with two\r\nbatteries under General Gregg, about two miles out of Raymond. This\r\nwas about two P.M. Logan was in advance with one of his brigades.\r\nHe deployed and moved up to engage the enemy. McPherson ordered the\r\nroad in rear to be cleared of wagons, and the balance of Logan's\r\ndivision, and Crocker's, which was still farther in rear, to come\r\nforward with all dispatch. The order was obeyed with alacrity.\r\nLogan got his division in position for assault before Crocker could\r\nget up, and attacked with vigor, carrying the enemy's position\r\neasily, sending Gregg flying from the field not to appear against\r\nour front again until we met at Jackson.", 'Paragraph 454': "In this battle McPherson lost 66 killed, 339 wounded, and 37\r\nmissing—nearly or quite all from Logan's division. The\r\nenemy's loss was 100 killed, 305 wounded, besides 415 taken\r\nprisoners.", 'Paragraph 455': 'I regarded Logan and Crocker as being as competent division\r\ncommanders as could be found in or out of the army and both equal\r\nto a much higher command. Crocker, however, was dying of\r\nconsumption when he volunteered. His weak condition never put him\r\non the sick report when there was a battle in prospect, as long as\r\nhe could keep on his feet. He died not long after the close of the\r\nrebellion.', 'Paragraph 456': "When the news reached me of McPherson's victory at Raymond about\r\nsundown my position was with Sherman. I decided at once to turn the\r\nwhole column towards Jackson and capture that place without\r\ndelay.", 'Paragraph 457': "Pemberton was now on my left, with, as I supposed, about 18,000\r\nmen; in fact, as I learned afterwards, with nearly 50,000. A force\r\nwas also collecting on my right, at Jackson, the point where all\r\nthe railroads communicating with Vicksburg connect. All the enemy's\r\nsupplies of men and stores would come by that point. As I hoped in\r\nthe end to besiege Vicksburg I must first destroy all possibility\r\nof aid. I therefore determined to move swiftly towards Jackson,\r\ndestroy or drive any force in that direction and then turn upon\r\nPemberton. But by moving against Jackson, I uncovered my own\r\ncommunication. So I finally decided to have none—to cut loose\r\naltogether from my base and move my whole force eastward. I then\r\nhad no fears for my communications, and if I moved quickly enough\r\ncould turn upon Pemberton before he could attack me in the\r\nrear.", 'Paragraph 458': "Accordingly, all previous orders given during the day for\r\nmovements on the 13th were annulled by new ones. McPherson was\r\nordered at daylight to move on Clinton, ten miles from Jackson;\r\nSherman was notified of my determination to capture Jackson and\r\nwork from there westward. He was ordered to start at four in the\r\nmorning and march to Raymond. McClernand was ordered to march with\r\nthree divisions by Dillon's to Raymond. One was left to guard the\r\ncrossing of the Big Black.", 'Paragraph 459': 'On the 10th I had received a letter from Banks, on the Red\r\nRiver, asking reinforcements. Porter had gone to his assistance\r\nwith a part of his fleet on the 3d, and I now wrote to him\r\ndescribing my position and declining to send any troops. I looked\r\nupon side movements as long as the enemy held Port Hudson and\r\nVicksburg as a waste of time and material.', 'Paragraph 460': 'General Joseph E. Johnston arrived at Jackson in the night of\r\nthe 13th from Tennessee, and immediately assumed command of all the\r\nConfederate troops in Mississippi. I knew he was expecting\r\nreinforcements from the south and east. On the 6th I had written to\r\nGeneral Halleck: "Information from the other side leaves me to\r\nbelieve the enemy are bringing forces from Tullahoma."', 'Paragraph 461': 'Up to this time my troops had been kept in supporting distances\r\nof each other, as far as the nature of the country would admit.\r\nReconnoissances were constantly made from each corps to enable them\r\nto acquaint themselves with the most practicable routes from one to\r\nanother in case a union became necessary.', 'Paragraph 462': "McPherson reached Clinton with the advance early on the 13th and\r\nimmediately set to work destroying the railroad. Sherman's advance\r\nreached Raymond before the last of McPherson's command had got out\r\nof the town. McClernand withdrew from the front of the enemy, at\r\nEdward's station, with much skill and without loss, and reached his\r\nposition for the night in good order. On the night of the 13th,\r\nMcPherson was ordered to march at early dawn upon Jackson, only\r\nfifteen miles away. Sherman was given the same order; but he was to\r\nmove by the direct road from Raymond to Jackson, which is south of\r\nthe road McPherson was on and does not approach within two miles of\r\nit at the point where it crossed the line of intrenchments which,\r\nat that time, defended the city. McClernand was ordered to move one\r\ndivision of his command to Clinton, one division a few miles beyond\r\nMississippi Springs following Sherman's line, and a third to\r\nRaymond. He was also directed to send his siege guns, four in\r\nnumber with the troops going by Mississippi Springs. McClernand's\r\nposition was an advantageous one in any event. With one division at\r\nClinton he was in position to reinforce McPherson, at Jackson,\r\nrapidly if it became necessary; the division beyond Mississippi\r\nSprings was equally available to reinforce Sherman; the one at\r\nRaymond could take either road. He still had two other divisions\r\nfarther back now that Blair had come up, available within a day at\r\nJackson. If this last command should not be wanted at Jackson, they\r\nwere already one day's march from there on their way to Vicksburg\r\nand on three different roads leading to the latter city. But the\r\nmost important consideration in my mind was to have a force\r\nconfronting Pemberton if he should come out to attack my rear. This\r\nI expected him to do; as shown further on, he was directed by\r\nJohnston to make this very move.", 'Paragraph 463': 'I notified General Halleck that I should attack the State\r\ncapital on the 14th. A courier carried the dispatch to Grand Gulf\r\nthrough an unprotected country.', 'Paragraph 464': "Sherman and McPherson communicated with each other during the\r\nnight and arranged to reach Jackson at about the same hour. It\r\nrained in torrents during the night of the 13th and the fore part\r\nof the day of the 14th. The roads were intolerable, and in some\r\nplaces on Sherman's line, where the land was low, they were covered\r\nmore than a foot deep with water. But the troops never murmured. By\r\nnine o'clock Crocker, of McPherson's corps, who was now in advance,\r\ncame upon the enemy's pickets and speedily drove them in upon the\r\nmain body. They were outside of the intrenchments in a strong\r\nposition, and proved to be the troops that had been driven out of\r\nRaymond. Johnston had been reinforced; during the night by Georgia\r\nand South Carolina regiments, so that his force amounted to eleven\r\nthousand men, and he was expecting still more.", 'Paragraph 465': "Sherman also came upon the rebel pickets some distance out from\r\nthe town, but speedily drove them in. He was now on the south and\r\nsouth-west of Jackson confronting the Confederates behind their\r\nbreastworks, while McPherson's right was nearly two miles north,\r\noccupying a line running north and south across the Vicksburg\r\nrailroad. Artillery was brought up and reconnoissances made\r\npreparatory to an assault. McPherson brought up Logan's division\r\nwhile he deployed Crocker's for the assault. Sherman made similar\r\ndispositions on the right. By eleven A.M. both were ready to\r\nattack. Crocker moved his division forward, preceded by a strong\r\nskirmish line. These troops at once encountered the enemy's advance\r\nand drove it back on the main body, when they returned to their\r\nproper regiment and the whole division charged, routing the enemy\r\ncompletely and driving him into this main line. This stand by the\r\nenemy was made more than two miles outside of his main\r\nfortifications. McPherson followed up with his command until within\r\nrange of the guns of the enemy from their intrenchments, when he\r\nhalted to bring his troops into line and reconnoitre to determine\r\nthe next move. It was now about noon.", 'Paragraph 466': "While this was going on Sherman was confronting a rebel battery\r\nwhich enfiladed the road on which he was marching—the\r\nMississippi Springs road—and commanded a bridge spanning a\r\nstream over which he had to pass. By detaching right and left the\r\nstream was forced and the enemy flanked and speedily driven within\r\nthe main line. This brought our whole line in front of the enemy's\r\nline of works, which was continuous on the north, west and south\r\nsides from the Pearl River north of the city to the same river\r\nsouth. I was with Sherman. He was confronted by a force sufficient\r\nto hold us back. Appearances did not justify an assault where we\r\nwere. I had directed Sherman to send a force to the right, and to\r\nreconnoitre as far as to the Pearl River. This force, Tuttle's\r\ndivision, not returning I rode to the right with my staff, and soon\r\nfound that the enemy had left that part of the line. Tuttle's\r\nmovement or McPherson's pressure had no doubt led Johnston to order\r\na retreat, leaving only the men at the guns to retard us while he\r\nwas getting away. Tuttle had seen this and, passing through the\r\nlines without resistance, came up in the rear of the artillerists\r\nconfronting Sherman and captured them with ten pieces of artillery.\r\nI rode immediately to the State House, where I was soon followed by\r\nSherman. About the same time McPherson discovered that the enemy\r\nwas leaving his front, and advanced Crocker, who was so close upon\r\nthe enemy that they could not move their guns or destroy them. He\r\ncaptured seven guns and, moving on, hoisted the National flag over\r\nthe rebel capital of Mississippi. Stevenson's brigade was sent to\r\ncut off the rebel retreat, but was too late or not expeditious\r\nenough.", 'Paragraph 467': 'Our loss in this engagement was: McPherson, 37 killed, 228\r\nwounded; Sherman, 4 killed and 21 wounded and missing. The enemy\r\nlost 845 killed, wounded and captured. Seventeen guns fell into our\r\nhands, and the enemy destroyed by fire their store-houses,\r\ncontaining a large amount of commissary stores.', 'Paragraph 468': "On this day Blair reached New Auburn and joined McClernand's 4th\r\ndivision. He had with him two hundred wagons loaded with rations,\r\nthe only commissary supplies received during the entire\r\ncampaign.", 'Paragraph 469': 'I slept that night in the room that Johnston was said to have\r\noccupied the night before.', 'Paragraph 470': 'About four in the afternoon I sent for the corps commanders and\r\ndirected the dispositions to be made of their troops. Sherman was\r\nto remain in Jackson until he destroyed that place as a railroad\r\ncentre, and manufacturing city of military supplies. He did the\r\nwork most effectually. Sherman and I went together into a\r\nmanufactory which had not ceased work on account of the battle nor\r\nfor the entrance of Yankee troops. Our presence did not seem to\r\nattract the attention of either the manager or the operatives, most\r\nof whom were girls. We looked on for a while to see the tent cloth\r\nwhich they were making roll out of the looms, with "C. S. A." woven\r\nin each bolt. There was an immense amount of cotton, in bales,\r\nstacked outside. Finally I told Sherman I thought they had done\r\nwork enough. The operatives were told they could leave and take\r\nwith them what cloth they could carry. In a few minutes cotton and\r\nfactory were in a blaze. The proprietor visited Washington while I\r\nwas President to get his pay for this property, claiming that it\r\nwas private. He asked me to give him a statement of the fact that\r\nhis property had been destroyed by National troops, so that he\r\nmight use it with Congress where he was pressing, or proposed to\r\npress, his claim. I declined.', 'Paragraph 471': 'On the night of the 13th Johnston sent the following dispatch to\r\nPemberton at Edward\'s station: "I have lately arrived, and learn\r\nthat Major-General Sherman is between us with four divisions at\r\nClinton. It is important to establish communication, that you may\r\nbe reinforced. If practicable, come up in his rear at once. To beat\r\nsuch a detachment would be of immense value. All the troops you can\r\nquickly assemble should be brought. Time is all-important." This\r\ndispatch was sent in triplicate, by different messengers. One of\r\nthe messengers happened to be a loyal man who had been expelled\r\nfrom Memphis some months before by Hurlbut for uttering disloyal\r\nand threatening sentiments. There was a good deal of parade about\r\nhis expulsion, ostensibly as a warning to those who entertained the\r\nsentiments he expressed; but Hurlbut and the expelled man\r\nunderstood each other. He delivered his copy of Johnston\'s dispatch\r\nto McPherson who forwarded it to me.', 'Paragraph 472': 'Receiving this dispatch on the 14th I ordered McPherson to move\r\npromptly in the morning back to Bolton, the nearest point where\r\nJohnston could reach the road. Bolton is about twenty miles west of\r\nJackson. I also informed McClernand of the capture of Jackson and\r\nsent him the following order: "It is evidently the design of the\r\nenemy to get north of us and cross the Big Black, and beat us into\r\nVicksburg. We must not allow them to do this. Turn all your forces\r\ntowards Bolton station, and make all dispatch in getting there.\r\nMove troops by the most direct road from wherever they may be on\r\nthe receipt of this order."', 'Paragraph 473': 'And to Blair I wrote: "Their design is evidently to cross the\r\nBig Black and pass down the peninsula between the Big Black and\r\nYazoo rivers. We must beat them. Turn your troops immediately to\r\nBolton; take all the trains with you. Smith\'s division, and any\r\nother troops now with you, will go to the same place. If\r\npracticable, take parallel roads, so as to divide your troops and\r\ntrain."', 'Paragraph 474': 'Johnston stopped on the Canton road only six miles north of\r\nJackson, the night of the 14th. He sent from there to Pemberton\r\ndispatches announcing the loss of Jackson, and the following\r\norder:', 'Paragraph 475': '"As soon as the reinforcements are all up, they must be united\r\nto the rest of the army. I am anxious to see a force assembled that\r\nmay be able to inflict a heavy blow upon the enemy. Can Grant\r\nsupply himself from the Mississippi? Can you not cut him off from\r\nit, and above all, should he be compelled to fall back for want of\r\nsupplies, beat him."', 'Paragraph 476': "The concentration of my troops was easy, considering the\r\ncharacter of the country. McPherson moved along the road parallel\r\nwith and near the railroad. McClernand's command was, one division\r\n(Hovey's) on the road McPherson had to take, but with a start of\r\nfour miles. One (Osterhaus) was at Raymond, on a converging road\r\nthat intersected the other near Champion's Hill; one (Carr's) had\r\nto pass over the same road with Osterhaus, but being back at\r\nMississippi Springs, would not be detained by it; the fourth\r\n(Smith's) with Blair's division, was near Auburn with a different\r\nroad to pass over. McClernand faced about and moved promptly. His\r\ncavalry from Raymond seized Bolton by half-past nine in the\r\nmorning, driving out the enemy's pickets and capturing several\r\nmen.", 'Paragraph 477': 'The night of the 15th Hovey was at Bolton; Carr and Osterhaus\r\nwere about three miles south, but abreast, facing west; Smith was\r\nnorth of Raymond with Blair in his rear.', 'Paragraph 478': "McPherson's command, with Logan in front, had marched at seven\r\no'clock, and by four reached Hovey and went into camp; Crocker\r\nbivouacked just in Hovey's rear on the Clinton road. Sherman with\r\ntwo divisions, was in Jackson, completing the destruction of roads,\r\nbridges and military factories. I rode in person out to Clinton. On\r\nmy arrival I ordered McClernand to move early in the morning on\r\nEdward's station, cautioning him to watch for the enemy and not\r\nbring on an engagement unless he felt very certain of success.", 'Paragraph 479': "I naturally expected that Pemberton would endeavor to obey the\r\norders of his superior, which I have shown were to attack us at\r\nClinton. This, indeed, I knew he could not do; but I felt sure he\r\nwould make the attempt to reach that point. It turned out, however,\r\nthat he had decided his superior's plans were impracticable, and\r\nconsequently determined to move south from Edward's station and get\r\nbetween me and my base. I, however, had no base, having abandoned\r\nit more than a week before. On the 15th Pemberton had actually\r\nmarched south from Edward's station, but the rains had swollen\r\nBaker's Creek, which he had to cross so much that he could not ford\r\nit, and the bridges were washed away. This brought him back to the\r\nJackson road, on which there was a good bridge over Baker's Creek.\r\nSome of his troops were marching until midnight to get there.\r\nReceiving here early on the 16th a repetition of his order to join\r\nJohnston at Clinton, he concluded to obey, and sent a dispatch to\r\nhis chief, informing him of the route by which he might be\r\nexpected.", 'Paragraph 480': "About five o'clock in the morning (16th) two men, who had been\r\nemployed on the Jackson and Vicksburg railroad, were brought to me.\r\nThey reported that they had passed through Pemberton's army in the\r\nnight, and that it was still marching east. They reported him to\r\nhave eighty regiments of infantry and ten batteries; in all, about\r\ntwenty-five thousand men.", 'Paragraph 481': "I had expected to leave Sherman at Jackson another day in order\r\nto complete his work; but getting the above information I sent him\r\norders to move with all dispatch to Bolton, and to put one division\r\nwith an ammunition train on the road at once, with directions to\r\nits commander to march with all possible speed until he came up to\r\nour rear. Within an hour after receiving this order Steele's\r\ndivision was on the road. At the same time I dispatched to Blair,\r\nwho was near Auburn, to move with all speed to Edward's station.\r\nMcClernand was directed to embrace Blair in his command for the\r\npresent. Blair's division was a part of the 15th army corps\r\n(Sherman's); but as it was on its way to join its corps, it\r\nnaturally struck our left first, now that we had faced about and\r\nwere moving west. The 15th corps, when it got up, would be on our\r\nextreme right. McPherson was directed to get his trains out of the\r\nway of the troops, and to follow Hovey's division as closely as\r\npossible. McClernand had two roads about three miles apart,\r\nconverging at Edward's station, over which to march his troops.\r\nHovey's division of his corps had the advance on a third road (the\r\nClinton) still farther north. McClernand was directed to move\r\nBlair's and A. J. Smith's divisions by the southernmost of these\r\nroads, and Osterhaus and Carr by the middle road. Orders were to\r\nmove cautiously with skirmishers to the front to feel for the\r\nenemy.", 'Paragraph 482': "Smith's division on the most southern road was the first to\r\nencounter the enemy's pickets, who were speedily driven in.\r\nOsterhaus, on the middle road, hearing the firing, pushed his\r\nskirmishers forward, found the enemy's pickets and forced them back\r\nto the main line. About the same time Hovey encountered the enemy\r\non the northern or direct wagon road from Jackson to Vicksburg.\r\nMcPherson was hastening up to join Hovey, but was embarrassed by\r\nHovey's trains occupying the roads. I was still back at Clinton.\r\nMcPherson sent me word of the situation, and expressed the wish\r\nthat I was up. By half-past seven I was on the road and proceeded\r\nrapidly to the front, ordering all trains that were in front of\r\ntroops off the road. When I arrived Hovey's skirmishing amounted\r\nalmost to a battle.", 'Paragraph 483': "McClernand was in person on the middle road and had a shorter\r\ndistance to march to reach the enemy's position than McPherson. I\r\nsent him word by a staff officer to push forward and attack. These\r\norders were repeated several times without apparently expediting\r\nMcClernand's advance.", 'Paragraph 484': "Champion's Hill, where Pemberton had chosen his position to\r\nreceive us, whether taken by accident or design, was well selected.\r\nIt is one of the highest points in that section, and commanded all\r\nthe ground in range. On the east side of the ridge, which is quite\r\nprecipitous, is a ravine running first north, then westerly,\r\nterminating at Baker's Creek. It was grown up thickly with large\r\ntrees and undergrowth, making it difficult to penetrate with\r\ntroops, even when not defended. The ridge occupied by the enemy\r\nterminated abruptly where the ravine turns westerly. The left of\r\nthe enemy occupied the north end of this ridge. The Bolton and\r\nEdward's station wagon-road turns almost due south at this point\r\nand ascends the ridge, which it follows for about a mile; then\r\nturning west, descends by a gentle declivity to Baker's Creek,\r\nnearly a mile away. On the west side the slope of the ridge is\r\ngradual and is cultivated from near the summit to the creek. There\r\nwas, when we were there, a narrow belt of timber near the summit\r\nwest of the road.", 'Paragraph 485': "From Raymond there is a direct road to Edward's station, some\r\nthree miles west of Champion's Hill. There is one also to Bolton.\r\nFrom this latter road there is still another, leaving it about\r\nthree and a half miles before reaching Bolton and leads direct to\r\nthe same station. It was along these two roads that three divisions\r\nof McClernand's corps, and Blair of Sherman's, temporarily under\r\nMcClernand, were moving. Hovey of McClernand's command was with\r\nMcPherson, farther north on the road from Bolton direct to Edward's\r\nstation. The middle road comes into the northern road at the point\r\nwhere the latter turns to the west and descends to Baker's Creek;\r\nthe southern road is still several miles south and does not\r\nintersect the others until it reaches Edward's station. Pemberton's\r\nlines covered all these roads, and faced east. Hovey's line, when\r\nit first drove in the enemy's pickets, was formed parallel to that\r\nof the enemy and confronted his left.", 'Paragraph 486': "By eleven o'clock the skirmishing had grown into a\r\nhard-contested battle. Hovey alone, before other troops could be\r\ngot to assist him, had captured a battery of the enemy. But he was\r\nnot able to hold his position and had to abandon the artillery.\r\nMcPherson brought up his troops as fast as possible, Logan in\r\nfront, and posted them on the right of Hovey and across the flank\r\nof the enemy. Logan reinforced Hovey with one brigade from his\r\ndivision; with his other two he moved farther west to make room for\r\nCrocker, who was coming up as rapidly as the roads would admit.\r\nHovey was still being heavily pressed, and was calling on me for\r\nmore reinforcements. I ordered Crocker, who was now coming up, to\r\nsend one brigade from his division. McPherson ordered two batteries\r\nto be stationed where they nearly enfiladed the enemy's line, and\r\nthey did good execution.", 'Paragraph 487': "From Logan's position now a direct forward movement carried him\r\nover open fields, in rear of the enemy and in a line parallel with\r\nthem. He did make exactly this move, attacking, however, the enemy\r\nthrough the belt of woods covering the west slope of the hill for a\r\nshort distance. Up to this time I had kept my position near Hovey\r\nwhere we were the most heavily pressed; but about noon I moved with\r\na part of my staff by our right around, until I came up with Logan\r\nhimself. I found him near the road leading down to Baker's Creek.\r\nHe was actually in command of the only road over which the enemy\r\ncould retreat; Hovey, reinforced by two brigades from McPherson's\r\ncommand, confronted the enemy's left; Crocker, with two brigades,\r\ncovered their left flank; McClernand two hours before, had been\r\nwithin two miles and a half of their centre with two divisions, and\r\nthe two divisions, Blair's and A. J. Smith's, were confronting the\r\nrebel right; Ransom, with a brigade of McArthur's division of the\r\n17th corps (McPherson's), had crossed the river at Grand Gulf a few\r\ndays before, and was coming up on their right flank. Neither Logan\r\nnor I knew that we had cut off the retreat of the enemy. Just at\r\nthis juncture a messenger came from Hovey, asking for more\r\nreinforcements. There were none to spare. I then gave an order to\r\nmove McPherson's command by the left flank around to Hovey. This\r\nuncovered the rebel line of retreat, which was soon taken advantage\r\nof by the enemy.", 'Paragraph 488': "During all this time, Hovey, reinforced as he was by a brigade\r\nfrom Logan and another from Crocker, and by Crocker gallantly\r\ncoming up with two other brigades on his right, had made several\r\nassaults, the last one about the time the road was opened to the\r\nrear. The enemy fled precipitately. This was between three and four\r\no'clock. I rode forward, or rather back, to where the middle road\r\nintersects the north road, and found the skirmishers of Carr's\r\ndivision just coming in. Osterhaus was farther south and soon after\r\ncame up with skirmishers advanced in like manner. Hovey's division,\r\nand McPherson's two divisions with him, had marched and fought from\r\nearly dawn, and were not in the best condition to follow the\r\nretreating foe. I sent orders to Osterhaus to pursue the enemy, and\r\nto Carr, whom I saw personally, I explained the situation and\r\ndirected him to pursue vigorously as far as the Big Black, and to\r\ncross it if he could; Osterhaus to follow him. The pursuit was\r\ncontinued until after dark.", 'Paragraph 489': "The battle of Champion's Hill lasted about four hours, hard\r\nfighting, preceded by two or three hours of skirmishing, some of\r\nwhich almost rose to the dignity of battle. Every man of Hovey's\r\ndivision and of McPherson's two divisions was engaged during the\r\nbattle. No other part of my command was engaged at all, except that\r\nas described before. Osterhaus's and A. J. Smith's divisions had\r\nencountered the rebel advanced pickets as early as half-past seven.\r\nTheir positions were admirable for advancing upon the enemy's line.\r\nMcClernand, with two divisions, was within a few miles of the\r\nbattle-field long before noon and in easy hearing. I sent him\r\nrepeated orders by staff officers fully competent to explain to him\r\nthe situation. These traversed the wood separating us, without\r\nescort, and directed him to push forward; but he did not come. It\r\nis true, in front of McClernand there was a small force of the\r\nenemy and posted in a good position behind a ravine obstructing his\r\nadvance; but if he had moved to the right by the road my staff\r\nofficers had followed the enemy must either have fallen back or\r\nbeen cut off. Instead of this he sent orders to Hovey, who belonged\r\nto his corps, to join on to his right flank. Hovey was bearing the\r\nbrunt of the battle at the time. To obey the order he would have\r\nhad to pull out from the front of the enemy and march back as far\r\nas McClernand had to advance to get into battle and substantially\r\nover the same ground. Of course I did not permit Hovey to obey the\r\norder of his intermediate superior.", 'Paragraph 490': "We had in this battle about 15,000 men absolutely engaged. This\r\nexcludes those that did not get up, all of McClernand's command\r\nexcept Hovey. Our loss was 410 killed, 1,844 wounded and 187\r\nmissing. Hovey alone lost 1,200 killed, wounded and\r\nmissing—more than one-third of his division.", 'Paragraph 491': "Had McClernand come up with reasonable promptness, or had I\r\nknown the ground as I did afterwards, I cannot see how Pemberton\r\ncould have escaped with any organized force. As it was he lost over\r\nthree thousand killed and wounded and about three thousand captured\r\nin battle and in pursuit. Loring's division, which was the right of\r\nPemberton's line, was cut off from the retreating army and never\r\ngot back into Vicksburg. Pemberton himself fell back that night to\r\nthe Big Black River. His troops did not stop before midnight and\r\nmany of them left before the general retreat commenced, and no\r\ndoubt a good part of them returned to their homes. Logan alone\r\ncaptured 1,300 prisoners and eleven guns. Hovey captured 300 under\r\nfire and about 700 in all, exclusive of 500 sick and wounded whom\r\nhe paroled, thus making 1,200.", 'Paragraph 492': "McPherson joined in the advance as soon as his men could fill\r\ntheir cartridge-boxes, leaving one brigade to guard our wounded.\r\nThe pursuit was continued as long as it was light enough to see the\r\nroad. The night of the 16th of May found McPherson's command\r\nbivouacked from two to six miles west of the battlefield, along the\r\nline of the road to Vicksburg. Carr and Osterhaus were at Edward's\r\nstation, and Blair was about three miles south-east; Hovey remained\r\non the field where his troops had fought so bravely and bled so\r\nfreely. Much war material abandoned by the enemy was picked up on\r\nthe battle-field, among it thirty pieces of artillery. I pushed\r\nthrough the advancing column with my staff and kept in advance\r\nuntil after night. Finding ourselves alone we stopped and took\r\npossession of a vacant house. As no troops came up we moved back a\r\nmile or more until we met the head of the column just going into\r\nbivouac on the road. We had no tents, so we occupied the porch of a\r\nhouse which had been taken for a rebel hospital and which was\r\nfilled with wounded and dying who had been brought from the\r\nbattle-field we had just left.", 'Paragraph 493': 'While a battle is raging one can see his enemy mowed down by the\r\nthousand, or the ten thousand, with great composure; but after the\r\nbattle these scenes are distressing, and one is naturally disposed\r\nto do as much to alleviate the suffering of an enemy as a\r\nfriend.', 'Paragraph 494': "We were now assured of our position between Johnston and\r\nPemberton, without a possibility of a junction of their forces.\r\nPemberton might have made a night march to the Big Black, crossed\r\nthe bridge there and, by moving north on the west side, have eluded\r\nus and finally returned to Johnston. But this would have given us\r\nVicksburg. It would have been his proper move, however, and the one\r\nJohnston would have made had he been in Pemberton's place. In fact\r\nit would have been in conformity with Johnston's orders to\r\nPemberton.", 'Paragraph 495': 'Sherman left Jackson with the last of his troops about noon on\r\nthe 16th and reached Bolton, twenty miles west, before halting. His\r\nrear guard did not get in until two A.M. the 17th, but renewed\r\ntheir march by daylight. He paroled his prisoners at Jackson, and\r\nwas forced to leave his own wounded in care of surgeons and\r\nattendants. At Bolton he was informed of our victory. He was\r\ndirected to commence the march early next day, and to diverge from\r\nthe road he was on to Bridgeport on the Big Black River, some\r\neleven miles above the point where we expected to find the enemy.\r\nBlair was ordered to join him there with the pontoon train as early\r\nas possible.', 'Paragraph 496': "This movement brought Sherman's corps together, and at a point\r\nwhere I hoped a crossing of the Big Black might be effected and\r\nSherman's corps used to flank the enemy out of his position in our\r\nfront, thus opening a crossing for the remainder of the army. I\r\ninformed him that I would endeavor to hold the enemy in my front\r\nwhile he crossed the river.", 'Paragraph 497': "The advance division, Carr's (McClernand's corps), resumed the\r\npursuit at half-past three A.M. on the 17th, followed closely by\r\nOsterhaus, McPherson bringing up the rear with his corps. As I\r\nexpected, the enemy was found in position on the Big Black. The\r\npoint was only six miles from that where my advance had rested for\r\nthe night, and was reached at an early hour. Here the river makes a\r\nturn to the west, and has washed close up to the high land; the\r\neast side is a low bottom, sometimes overflowed at very high water,\r\nbut was cleared and in cultivation. A bayou runs irregularly across\r\nthis low land, the bottom of which, however, is above the surface\r\nof the Big Black at ordinary stages. When the river is full water\r\nruns through it, converting the point of land into an island. The\r\nbayou was grown up with timber, which the enemy had felled into the\r\nditch. At this time there was a foot or two of water in it. The\r\nrebels had constructed a parapet along the inner bank of this bayou\r\nby using cotton bales from the plantation close by and throwing\r\ndirt over them. The whole was thoroughly commanded from the height\r\nwest of the river. At the upper end of the bayou there was a strip\r\nof uncleared land which afforded a cover for a portion of our men.\r\nCarr's division was deployed on our right, Lawler's brigade forming\r\nhis extreme right and reaching through these woods to the river\r\nabove. Osterhaus' division was deployed to the left of Carr and\r\ncovered the enemy's entire front. McPherson was in column on the\r\nroad, the head close by, ready to come in wherever he could be of\r\nassistance.", 'Paragraph 498': "While the troops were standing as here described an officer from\r\nBanks' staff came up and presented me with a letter from General\r\nHalleck, dated the 11th of May. It had been sent by the way of New\r\nOrleans to Banks to be forwarded to me. It ordered me to return to\r\nGrand Gulf and to co-operate from there with Banks against Port\r\nHudson, and then to return with our combined forces to besiege\r\nVicksburg. I told the officer that the order came too late, and\r\nthat Halleck would not give it now if he knew our position. The\r\nbearer of the dispatch insisted that I ought to obey the order, and\r\nwas giving arguments to support his position when I heard great\r\ncheering to the right of our line and, looking in that direction,\r\nsaw Lawler in his shirt sleeves leading a charge upon the enemy. I\r\nimmediately mounted my horse and rode in the direction of the\r\ncharge, and saw no more of the officer who delivered the dispatch;\r\nI think not even to this day.", 'Paragraph 499': 'The assault was successful. But little resistance was made. The\r\nenemy fled from the west bank of the river, burning the bridge\r\nbehind him and leaving the men and guns on the east side to fall\r\ninto our hands. Many tried to escape by swimming the river. Some\r\nsucceeded and some were drowned in the attempt. Eighteen guns were\r\ncaptured and 1,751 prisoners. Our loss was 39 killed, 237 wounded\r\nand 3 missing. The enemy probably lost but few men except those\r\ncaptured and drowned. But for the successful and complete\r\ndestruction of the bridge, I have but little doubt that we should\r\nhave followed the enemy so closely as to prevent his occupying his\r\ndefences around Vicksburg.', 'Paragraph 500': "As the bridge was destroyed and the river was high, new bridges\r\nhad to be built. It was but little after nine o'clock A.M. when the\r\ncapture took place. As soon as work could be commenced, orders were\r\ngiven for the construction of three bridges. One was taken charge\r\nof by Lieutenant Hains, of the Engineer Corps, one by General\r\nMcPherson himself and one by General Ransom, a most gallant and\r\nintelligent volunteer officer. My recollection is that Hains built\r\na raft bridge; McPherson a pontoon, using cotton bales in large\r\nnumbers, for pontoons; and that Ransom felled trees on opposite\r\nbanks of the river, cutting only on one side of the tree, so that\r\nthey would fall with their tops interlacing in the river, without\r\nthe trees being entirely severed from their stumps. A bridge was\r\nthen made with these trees to support the roadway. Lumber was taken\r\nfrom buildings, cotton gins and wherever found, for this purpose.\r\nBy eight o'clock in the morning of the 18th all three bridges were\r\ncomplete and the troops were crossing.", 'Paragraph 501': 'Sherman reached Bridgeport about noon of the 17th and found\r\nBlair with the pontoon train already there. A few of the enemy were\r\nintrenched on the west bank, but they made little resistance and\r\nsoon surrendered. Two divisions were crossed that night and the\r\nthird the following morning.', 'Paragraph 502': "On the 18th I moved along the Vicksburg road in advance of the\r\ntroops and as soon as possible joined Sherman. My first anxiety was\r\nto secure a base of supplies on the Yazoo River above Vicksburg.\r\nSherman's line of march led him to the very point on Walnut Hills\r\noccupied by the enemy the December before when he was repulsed.\r\nSherman was equally anxious with myself. Our impatience led us to\r\nmove in advance of the column and well up with the advanced\r\nskirmishers. There were some detached works along the crest of the\r\nhill. These were still occupied by the enemy, or else the garrison\r\nfrom Haines' Bluff had not all got past on their way to Vicksburg.\r\nAt all events the bullets of the enemy whistled by thick and fast\r\nfor a short time. In a few minutes Sherman had the pleasure of\r\nlooking down from the spot coveted so much by him the December\r\nbefore on the ground where his command had lain so helpless for\r\noffensive action. He turned to me, saying that up to this minute he\r\nhad felt no positive assurance of success. This, however, he said\r\nwas the end of one of the greatest campaigns in history and I ought\r\nto make a report of it at once. Vicksburg was not yet captured, and\r\nthere was no telling what might happen before it was taken; but\r\nwhether captured or not, this was a complete and successful\r\ncampaign. I do not claim to quote Sherman's language; but the\r\nsubstance only. My reason for mentioning this incident will appear\r\nfurther on.", 'Paragraph 503': "McPherson, after crossing the Big Black, came into the Jackson\r\nand Vicksburg road which Sherman was on, but to his rear. He\r\narrived at night near the lines of the enemy, and went into camp.\r\nMcClernand moved by the direct road near the railroad to Mount\r\nAlbans, and then turned to the left and put his troops on the road\r\nfrom Baldwin's ferry to Vicksburg. This brought him south of\r\nMcPherson. I now had my three corps up the works built for the\r\ndefence of Vicksburg, on three roads—one to the north, one to\r\nthe east and one to the south-east of the city. By the morning of\r\nthe 19th the investment was as complete as my limited number of\r\ntroops would allow. Sherman was on the right, and covered the high\r\nground from where it overlooked the Yazoo as far south-east as his\r\ntroops would extend. McPherson joined on to his left, and occupied\r\nground on both sides of the Jackson road. McClernand took up the\r\nground to his left and extended as far towards Warrenton as he\r\ncould, keeping a continuous line.", 'Paragraph 504': "On the 19th there was constant skirmishing with the enemy while\r\nwe were getting into better position. The enemy had been much\r\ndemoralized by his defeats at Champion's Hill and the Big Black,\r\nand I believed he would not make much effort to hold Vicksburg.\r\nAccordingly, at two o'clock I ordered an assault. It resulted in\r\nsecuring more advanced positions for all our troops where they were\r\nfully covered from the fire of the enemy.", 'Paragraph 505': 'The 20th and 21st were spent in strengthening our position and\r\nin making roads in rear of the army, from Yazoo River or Chickasaw\r\nBayou. Most of the army had now been for three weeks with only five\r\ndays\' rations issued by the commissary. They had an abundance of\r\nfood, however, but began to feel the want of bread. I remember that\r\nin passing around to the left of the line on the 21st, a soldier,\r\nrecognizing me, said in rather a low voice, but yet so that I heard\r\nhim, "Hard tack." In a moment the cry was taken up all along the\r\nline, "Hard tack! Hard tack!" I told the men nearest to me that we\r\nhad been engaged ever since the arrival of the troops in building a\r\nroad over which to supply them with everything they needed. The cry\r\nwas instantly changed to cheers. By the night of the 21st all the\r\ntroops had full rations issued to them. The bread and coffee were\r\nhighly appreciated.', 'Paragraph 506': 'I now determined on a second assault. Johnston was in my rear,\r\nonly fifty miles away, with an army not much inferior in numbers to\r\nthe one I had with me, and I knew he was being reinforced. There\r\nwas danger of his coming to the assistance of Pemberton, and after\r\nall he might defeat my anticipations of capturing the garrison if,\r\nindeed, he did not prevent the capture of the city. The immediate\r\ncapture of Vicksburg would save sending me the reinforcements which\r\nwere so much wanted elsewhere, and would set free the army under me\r\nto drive Johnston from the State. But the first consideration of\r\nall was—the troops believed they could carry the works in\r\ntheir front, and would not have worked so patiently in the trenches\r\nif they had not been allowed to try.', 'Paragraph 507': "The attack was ordered to commence on all parts of the line at\r\nten o'clock A.M. on the 22d with a furious cannonade from every\r\nbattery in position. All the corps commanders set their time by\r\nmine so that all might open the engagement at the same minute. The\r\nattack was gallant, and portions of each of the three corps\r\nsucceeded in getting up to the very parapets of the enemy and in\r\nplanting their battle flags upon them; but at no place were we able\r\nto enter. General McClernand reported that he had gained the\r\nenemy's intrenchments at several points, and wanted reinforcements.\r\nI occupied a position from which I believed I could see as well as\r\nhe what took place in his front, and I did not see the success he\r\nreported. But his request for reinforcements being repeated I could\r\nnot ignore it, and sent him Quinby's division of the 17th corps.\r\nSherman and McPherson were both ordered to renew their assaults as\r\na diversion in favor of McClernand. This last attack only served to\r\nincrease our casualties without giving any benefit whatever. As\r\nsoon as it was dark our troops that had reached the enemy's line\r\nand been obliged to remain there for security all day, were\r\nwithdrawn; and thus ended the last assault upon Vicksburg.", 'Paragraph 508': 'I now determined upon a regular siege—to "out-camp the\r\nenemy," as it were, and to incur no more losses. The experience of\r\nthe 22d convinced officers and men that this was best, and they\r\nwent to work on the defences and approaches with a will. With the\r\nnavy holding the river, the investment of Vicksburg was complete.\r\nAs long as we could hold our position the enemy was limited in\r\nsupplies of food, men and munitions of war to what they had on\r\nhand. These could not last always.', 'Paragraph 509': "The crossing of troops at Bruinsburg commenced April 30th. On\r\nthe 18th of May the army was in rear of Vicksburg. On the 19th,\r\njust twenty days after the crossing, the city was completely\r\ninvested and an assault had been made: five distinct battles\r\n(besides continuous skirmishing) had been fought and won by the\r\nUnion forces; the capital of the State had fallen and its arsenals,\r\nmilitary manufactories and everything useful for military purposes\r\nhad been destroyed; an average of about one hundred and eighty\r\nmiles had been marched by the troops engaged; but five days'\r\nrations had been issued, and no forage; over six thousand prisoners\r\nhad been captured, and as many more of the enemy had been killed or\r\nwounded; twenty-seven heavy cannon and sixty-one field-pieces had\r\nfallen into our hands; and four hundred miles of the river, from\r\nVicksburg to Port Hudson, had become ours. The Union force that had\r\ncrossed the Mississippi River up to this time was less than\r\nforty-three thousand men. One division of these, Blair's, only\r\narrived in time to take part in the battle of Champion's Hill, but\r\nwas not engaged there; and one brigade, Ransom's of McPherson's\r\ncorps, reached the field after the battle. The enemy had at\r\nVicksburg, Grand Gulf, Jackson, and on the roads between these\r\nplaces, over sixty thousand men. They were in their own country,\r\nwhere no rear guards were necessary. The country is admirable for\r\ndefence, but difficult for the conduct of an offensive campaign.\r\nAll their troops had to be met. We were fortunate, to say the\r\nleast, in meeting them in detail: at Port Gibson seven or eight\r\nthousand; at Raymond, five thousand; at Jackson, from eight to\r\neleven thousand; at Champion's Hill, twenty-five thousand; at the\r\nBig Black, four thousand. A part of those met at Jackson were all\r\nthat was left of those encountered at Raymond. They were beaten in\r\ndetail by a force smaller than their own, upon their own ground.\r\nOur loss up to this time was:", 'Paragraph 510': 'Of the wounded many were but slightly so, and continued on duty.\r\nNot half of them were disabled for any length of time.', 'Paragraph 511': "After the unsuccessful assault of the 22d the work of the\r\nregular siege began. Sherman occupied the right starting from the\r\nriver above Vicksburg, McPherson the centre (McArthur's division\r\nnow with him) and McClernand the left, holding the road south to\r\nWarrenton. Lauman's division arrived at this time and was placed on\r\nthe extreme left of the line.", 'Paragraph 512': 'In the interval between the assaults of the 19th and 22d, roads\r\nhad been completed from the Yazoo River and Chickasaw Bayou, around\r\nthe rear of the army, to enable us to bring up supplies of food and\r\nammunition; ground had been selected and cleared on which the\r\ntroops were to be encamped, and tents and cooking utensils were\r\nbrought up. The troops had been without these from the time of\r\ncrossing the Mississippi up to this time. All was now ready for the\r\npick and spade. Prentiss and Hurlbut were ordered to send forward\r\nevery man that could be spared. Cavalry especially was wanted to\r\nwatch the fords along the Big Black, and to observe Johnston. I\r\nknew that Johnston was receiving reinforcements from Bragg, who was\r\nconfronting Rosecrans in Tennessee. Vicksburg was so important to\r\nthe enemy that I believed he would make the most strenuous efforts\r\nto raise the siege, even at the risk of losing ground\r\nelsewhere.', 'Paragraph 513': "My line was more than fifteen miles long, extending from Haines'\r\nBluff to Vicksburg, thence to Warrenton. The line of the enemy was\r\nabout seven. In addition to this, having an enemy at Canton and\r\nJackson, in our rear, who was being constantly reinforced, we\r\nrequired a second line of defence facing the other way. I had not\r\ntroops enough under my command to man these. General Halleck\r\nappreciated the situation and, without being asked, forwarded\r\nreinforcements with all possible dispatch.", 'Paragraph 514': "The ground about Vicksburg is admirable for defence. On the\r\nnorth it is about two hundred feet above the Mississippi River at\r\nthe highest point and very much cut up by the washing rains; the\r\nravines were grown up with cane and underbrush, while the sides and\r\ntops were covered with a dense forest. Farther south the ground\r\nflattens out somewhat, and was in cultivation. But here, too, it\r\nwas cut up by ravines and small streams. The enemy's line of\r\ndefence followed the crest of a ridge from the river north of the\r\ncity eastward, then southerly around to the Jackson road, full\r\nthree miles back of the city; thence in a southwesterly direction\r\nto the river. Deep ravines of the description given lay in front of\r\nthese defences. As there is a succession of gullies, cut out by\r\nrains along the side of the ridge, the line was necessarily very\r\nirregular. To follow each of these spurs with intrenchments, so as\r\nto command the slopes on either side, would have lengthened their\r\nline very much. Generally therefore, or in many places, their line\r\nwould run from near the head of one gully nearly straight to the\r\nhead of another, and an outer work triangular in shape, generally\r\nopen in the rear, was thrown up on the point; with a few men in\r\nthis outer work they commanded the approaches to the main line\r\ncompletely.", 'Paragraph 515': 'The work to be done, to make our position as strong against the\r\nenemy as his was against us, was very great. The problem was also\r\ncomplicated by our wanting our line as near that of the enemy as\r\npossible. We had but four engineer officers with us. Captain Prime,\r\nof the Engineer Corps, was the chief, and the work at the beginning\r\nwas mainly directed by him. His health soon gave out, when he was\r\nsucceeded by Captain Comstock, also of the Engineer Corps. To\r\nprovide assistants on such a long line I directed that all officers\r\nwho had graduated at West Point, where they had necessarily to\r\nstudy military engineering, should in addition to their other\r\nduties assist in the work.', 'Paragraph 516': 'The chief quartermaster and the chief commissary were graduates.\r\nThe chief commissary, now the Commissary-General of the Army,\r\nbegged off, however, saying that there was nothing in engineering\r\nthat he was good for unless he would do for a sap-roller. As\r\nsoldiers require rations while working in the ditches as well as\r\nwhen marching and fighting, and as we would be sure to lose him if\r\nhe was used as a sap-roller, I let him off. The general is a large\r\nman; weighs two hundred and twenty pounds, and is not tall.', 'Paragraph 517': 'We had no siege guns except six thirty-two pounders, and there\r\nwere none at the West to draw from. Admiral Porter, however,\r\nsupplied us with a battery of navy-guns of large calibre, and with\r\nthese, and the field artillery used in the campaign, the siege\r\nbegan. The first thing to do was to get the artillery in batteries\r\nwhere they would occupy commanding positions; then establish the\r\ncamps, under cover from the fire of the enemy but as near up as\r\npossible; and then construct rifle-pits and covered ways, to\r\nconnect the entire command by the shortest route. The enemy did not\r\nharass us much while we were constructing our batteries. Probably\r\ntheir artillery ammunition was short; and their infantry was kept\r\ndown by our sharpshooters, who were always on the alert and ready\r\nto fire at a head whenever it showed itself above the rebel\r\nworks.', 'Paragraph 518': 'In no place were our lines more than six hundred yards from the\r\nenemy. It was necessary, therefore, to cover our men by something\r\nmore than the ordinary parapet. To give additional protection sand\r\nbags, bullet-proof, were placed along the tops of the parapets far\r\nenough apart to make loop-holes for musketry. On top of these, logs\r\nwere put. By these means the men were enabled to walk about erect\r\nwhen off duty, without fear of annoyance from sharpshooters. The\r\nenemy used in their defence explosive musket-balls, no doubt\r\nthinking that, bursting over our men in the trenches, they would do\r\nsome execution; but I do not remember a single case where a man was\r\ninjured by a piece of one of these shells. When they were hit and\r\nthe ball exploded, the wound was terrible. In these cases a solid\r\nball would have hit as well. Their use is barbarous, because they\r\nproduce increased suffering without any corresponding advantage to\r\nthose using them.', 'Paragraph 519': 'The enemy could not resort to our method to protect their men,\r\nbecause we had an inexhaustible supply of ammunition to draw upon\r\nand used it freely. Splinters from the timber would have made havoc\r\namong the men behind.', 'Paragraph 520': 'There were no mortars with the besiegers, except what the navy\r\nhad in front of the city; but wooden ones were made by taking logs\r\nof the toughest wood that could be found, boring them out for six\r\nor twelve pound shells and binding them with strong iron bands.\r\nThese answered as coehorns, and shells were successfully thrown\r\nfrom them into the trenches of the enemy.', 'Paragraph 521': "The labor of building the batteries and intrenching was largely\r\ndone by the pioneers, assisted by negroes who came within our lines\r\nand who were paid for their work; but details from the troops had\r\noften to be made. The work was pushed forward as rapidly as\r\npossible, and when an advanced position was secured and covered\r\nfrom the fire of the enemy the batteries were advanced. By the 30th\r\nof June there were two hundred and twenty guns in position, mostly\r\nlight field-pieces, besides a battery of heavy guns belonging to,\r\nmanned and commanded by the navy. We were now as strong for defence\r\nagainst the garrison of Vicksburg as they were against us; but I\r\nknew that Johnston was in our rear, and was receiving constant\r\nreinforcements from the east. He had at this time a larger force\r\nthan I had had at any time prior to the battle of Champion's\r\nHill.", 'Paragraph 522': 'As soon as the news of the arrival of the Union army behind\r\nVicksburg reached the North, floods of visitors began to pour in.\r\nSome came to gratify curiosity; some to see sons or brothers who\r\nhad passed through the terrible ordeal; members of the Christian\r\nand Sanitary Associations came to minister to the wants of the sick\r\nand the wounded. Often those coming to see a son or brother would\r\nbring a dozen or two of poultry. They did not know how little the\r\ngift would be appreciated. Many of the soldiers had lived so much\r\non chickens, ducks and turkeys without bread during the march, that\r\nthe sight of poultry, if they could get bacon, almost took away\r\ntheir appetite. But the intention was good.', 'Paragraph 523': 'Among the earliest arrivals was the Governor of Illinois, with\r\nmost of the State officers. I naturally wanted to show them what\r\nthere was of most interest. In Sherman\'s front the ground was the\r\nmost broken and most wooded, and more was to be seen without\r\nexposure. I therefore took them to Sherman\'s headquarters and\r\npresented them. Before starting out to look at the\r\nlines—possibly while Sherman\'s horse was being\r\nsaddled—there were many questions asked about the late\r\ncampaign, about which the North had been so imperfectly informed.\r\nThere was a little knot around Sherman and another around me, and I\r\nheard Sherman repeating, in the most animated manner, what he had\r\nsaid to me when we first looked down from Walnut Hills upon the\r\nland below on the 18th of May, adding: "Grant is entitled to every\r\nbit of the credit for the campaign; I opposed it. I wrote him a\r\nletter about it." But for this speech it is not likely that\r\nSherman\'s opposition would have ever been heard of. His untiring\r\nenergy and great efficiency during the campaign entitle him to a\r\nfull share of all the credit due for its success. He could not have\r\ndone more if the plan had been his own.', 'Paragraph 524': "[NOTE.—When General Sherman first learned of the\r\nmove I proposed to make, he called to see me about it. I recollect\r\nthat I had transferred my headquarters from a boat in the river to\r\na house a short distance back from the levee. I was seated on the\r\npiazza engaged in conversation with my staff when Sherman came up.\r\nAfter a few moments' conversation he said that he would like to see\r\nme alone. We passed into the house together and shut the door after\r\nus. Sherman then expressed his alarm at the move I had ordered,\r\nsaying that I was putting myself in a position voluntarily which an\r\nenemy would be glad to manoeuvre a year—or a long\r\ntime—to get me in. I was going into the enemy's country, with\r\na large river behind me and the enemy holding points strongly\r\nfortified above and below. He said that it was an axiom in war that\r\nwhen any great body of troops moved against an enemy they should do\r\nso from a base of supplies, which they would guard as they would\r\nthe apple of the eye, etc. He pointed out all the difficulties that\r\nmight be encountered in the campaign proposed, and stated in turn\r\nwhat would be the true campaign to make. This was, in substance, to\r\ngo back until high ground could be reached on the east bank of the\r\nriver; fortify there and establish a depot of supplies, and move\r\nfrom there, being always prepared to fall back upon it in case of\r\ndisaster. I said this would take us back to Memphis. Sherman then\r\nsaid that was the very place he would go to, and would move by\r\nrailroad from Memphis to Grenada, repairing the road as we\r\nadvanced. To this I replied, the country is already disheartened\r\nover the lack of success on the part of our armies; the last\r\nelection went against the vigorous prosecution of the war,\r\nvoluntary enlistments had ceased throughout most of the North and\r\nconscription was already resorted to, and if we went back so far as\r\nMemphis it would discourage the people so much that bases of\r\nsupplies would be of no use: neither men to hold them nor supplies\r\nto put in them would be furnished. The problem for us was to move\r\nforward to a decisive victory, or our cause was lost. No progress\r\nwas being made in any other field, and we had to go on. Sherman\r\nwrote to my adjutant general, Colonel J. A. Rawlins, embodying his\r\nviews of the campaign that should be made, and asking him to advise\r\nme to at least get the views of my generals upon the subject.\r\nColonel Rawlins showed me the letter, but I did not see any reason\r\nfor changing my plans. The letter was not answered and the subect\r\nwas not subsequently mentioned between Sherman and myself to the\r\nend of the war, that I remember of. I did not regard the letter as\r\nofficial, and consequently did not preserve it. General Sherman\r\nfurnished a copy himself to General Badeau, who printed it in his\r\nhistory of my campaigns. I did not regard either the conversation\r\nbetween us or the letter to my adjutant-general as protests, but\r\nsimply friendly advice which the relations between us fully\r\njustified. Sherman gave the same energy to make the campaign a\r\nsuccess that he would or could have done if it had been ordered by\r\nhimself. I make this statement here to correct an impression which\r\nwas circulated at the close of the war to Sherman's prejudice, and\r\nfor which there was no fair foundation.]", 'Paragraph 525': "On the 26th of May I sent Blair's division up the Yazoo to drive\r\nout a force of the enemy supposed to be between the Big Black and\r\nthe Yazoo. The country was rich and full of supplies of both food\r\nand forage. Blair was instructed to take all of it. The cattle were\r\nto be driven in for the use of our army, and the food and forage to\r\nbe consumed by our troops or destroyed by fire; all bridges were to\r\nbe destroyed, and the roads rendered as nearly impassable as\r\npossible. Blair went forty-five miles and was gone almost a week.\r\nHis work was effectually done. I requested Porter at this time to\r\nsend the marine brigade, a floating nondescript force which had\r\nbeen assigned to his command and which proved very useful, up to\r\nHaines' Bluff to hold it until reinforcements could be sent.", 'Paragraph 526': 'On the 26th I also received a letter from Banks, asking me to\r\nreinforce him with ten thousand men at Port Hudson. Of course I\r\ncould not comply with his request, nor did I think he needed them.\r\nHe was in no danger of an attack by the garrison in his front, and\r\nthere was no army organizing in his rear to raise the siege.', 'Paragraph 527': "On the 3d of June a brigade from Hurlbut's command arrived,\r\nGeneral Kimball commanding. It was sent to Mechanicsburg, some\r\nmiles north-east of Haines' Bluff and about midway between the Big\r\nBlack and the Yazoo. A brigade of Blair's division and twelve\r\nhundred cavalry had already, on Blair's return from the Yazoo, been\r\nsent to the same place with instructions to watch the crossings of\r\nthe Big Black River, to destroy the roads in his (Blair's) front,\r\nand to gather or destroy all supplies.", 'Paragraph 528': "On the 7th of June our little force of colored and white troops\r\nacross the Mississippi, at Milliken's Bend, were attacked by about\r\n3,000 men from Richard Taylor's trans-Mississippi command. With the\r\naid of the gunboats they were speedily repelled. I sent Mower's\r\nbrigade over with instructions to drive the enemy beyond the Tensas\r\nBayou; and we had no further trouble in that quarter during the\r\nsiege. This was the first important engagement of the war in which\r\ncolored troops were under fire. These men were very raw, having all\r\nbeen enlisted since the beginning of the siege, but they behaved\r\nwell.", 'Paragraph 529': "On the 8th of June a full division arrived from Hurlbut's\r\ncommand, under General Sooy Smith. It was sent immediately to\r\nHaines' Bluff, and General C. C. Washburn was assigned to the\r\ngeneral command at that point.", 'Paragraph 530': "On the 11th a strong division arrived from the Department of the\r\nMissouri under General Herron, which was placed on our left. This\r\ncut off the last possible chance of communication between Pemberton\r\nand Johnston, as it enabled Lauman to close up on McClernand's left\r\nwhile Herron intrenched from Lauman to the water's edge. At this\r\npoint the water recedes a few hundred yards from the high land.\r\nThrough this opening no doubt the Confederate commanders had been\r\nable to get messengers under cover of night.", 'Paragraph 531': "On the 14th General Parke arrived with two divisions of\r\nBurnside's corps, and was immediately dispatched to Haines' Bluff.\r\nThese latter troops—Herron's and Parke's—were the\r\nreinforcements already spoken of sent by Halleck in anticipation of\r\ntheir being needed. They arrived none too soon.", 'Paragraph 532': "I now had about seventy-one thousand men. More than half were\r\ndisposed across the peninsula, between the Yazoo at Haines' Bluff\r\nand the Big Black, with the division of Osterhaus watching the\r\ncrossings of the latter river farther south and west from the\r\ncrossing of the Jackson road to Baldwin's ferry and below.", 'Paragraph 533': 'There were eight roads leading into Vicksburg, along which and\r\ntheir immediate sides, our work was specially pushed and batteries\r\nadvanced; but no commanding point within range of the enemy was\r\nneglected.', 'Paragraph 534': "On the 17th I received a letter from General Sherman and one on\r\nthe 18th from General McPherson, saying that their respective\r\ncommands had complained to them of a fulsome, congratulatory order\r\npublished by General McClernand to the 13th corps, which did great\r\ninjustice to the other troops engaged in the campaign. This order\r\nhad been sent North and published, and now papers containing it had\r\nreached our camps. The order had not been heard of by me, and\r\ncertainly not by troops outside of McClernand's command until\r\nbrought in this way. I at once wrote to McClernand, directing him\r\nto send me a copy of this order. He did so, and I at once relieved\r\nhim from the command of the 13th army corps and ordered him back to\r\nSpringfield, Illinois. The publication of his order in the press\r\nwas in violation of War Department orders and also of mine.", 'Paragraph 535': "On the 22d of June positive information was received that\r\nJohnston had crossed the Big Black River for the purpose of\r\nattacking our rear, to raise the siege and release Pemberton. The\r\ncorrespondence between Johnston and Pemberton shows that all\r\nexpectation of holding Vicksburg had by this time passed from\r\nJohnston's mind. I immediately ordered Sherman to the command of\r\nall the forces from Haines' Bluff to the Big Black River. This\r\namounted now to quite half the troops about Vicksburg. Besides\r\nthese, Herron and A. J. Smith's divisions were ordered to hold\r\nthemselves in readiness to reinforce Sherman. Haines' Bluff had\r\nbeen strongly fortified on the land side, and on all commanding\r\npoints from there to the Big Black at the railroad crossing\r\nbatteries had been constructed. The work of connecting by\r\nrifle-pits where this was not already done, was an easy task for\r\nthe troops that were to defend them.", 'Paragraph 536': "We were now looking west, besieging Pemberton, while we were\r\nalso looking east to defend ourselves against an expected siege by\r\nJohnston. But as against the garrison of Vicksburg we were as\r\nsubstantially protected as they were against us. Where we were\r\nlooking east and north we were strongly fortified, and on the\r\ndefensive. Johnston evidently took in the situation and wisely, I\r\nthink, abstained from making an assault on us because it would\r\nsimply have inflicted loss on both sides without accomplishing any\r\nresult. We were strong enough to have taken the offensive against\r\nhim; but I did not feel disposed to take any risk of losing our\r\nhold upon Pemberton's army, while I would have rejoiced at the\r\nopportunity of defending ourselves against an attack by\r\nJohnston.", 'Paragraph 537': "From the 23d of May the work of fortifying and pushing forward\r\nour position nearer to the enemy had been steadily progressing. At\r\nthree points on the Jackson road, in front of Leggett's brigade, a\r\nsap was run up to the enemy's parapet, and by the 25th of June we\r\nhad it undermined and the mine charged. The enemy had countermined,\r\nbut did not succeed in reaching our mine. At this particular point\r\nthe hill on which the rebel work stands rises abruptly. Our sap ran\r\nclose up to the outside of the enemy's parapet. In fact this\r\nparapet was also our protection. The soldiers of the two sides\r\noccasionally conversed pleasantly across this barrier; sometimes\r\nthey exchanged the hard bread of the Union soldiers for the tobacco\r\nof the Confederates; at other times the enemy threw over\r\nhand-grenades, and often our men, catching them in their hands,\r\nreturned them.", 'Paragraph 538': 'Our mine had been started some distance back down the hill;\r\nconsequently when it had extended as far as the parapet it was many\r\nfeet below it. This caused the failure of the enemy in his search\r\nto find and destroy it. On the 25th of June at three o\'clock, all\r\nbeing ready, the mine was exploded. A heavy artillery fire all\r\nalong the line had been ordered to open with the explosion. The\r\neffect was to blow the top of the hill off and make a crater where\r\nit stood. The breach was not sufficient to enable us to pass a\r\ncolumn of attack through. In fact, the enemy having failed to reach\r\nour mine had thrown up a line farther back, where most of the men\r\nguarding that point were placed. There were a few men, however,\r\nleft at the advance line, and others working in the countermine,\r\nwhich was still being pushed to find ours. All that were there were\r\nthrown into the air, some of them coming down on our side, still\r\nalive. I remember one colored man, who had been under ground at\r\nwork when the explosion took place, who was thrown to our side. He\r\nwas not much hurt, but terribly frightened. Some one asked him how\r\nhigh he had gone up. "Dun no, massa, but t\'ink \'bout t\'ree mile,"\r\nwas his reply. General Logan commanded at this point and took this\r\ncolored man to his quarters, where he did service to the end of the\r\nsiege.', 'Paragraph 539': 'As soon as the explosion took place the crater was seized by two\r\nregiments of our troops who were near by, under cover, where they\r\nhad been placed for the express purpose. The enemy made a desperate\r\neffort to expel them, but failed, and soon retired behind the new\r\nline. From here, however, they threw hand-grenades, which did some\r\nexecution. The compliment was returned by our men, but not with so\r\nmuch effect. The enemy could lay their grenades on the parapet,\r\nwhich alone divided the contestants, and roll them down upon us;\r\nwhile from our side they had to be thrown over the parapet, which\r\nwas at considerable elevation. During the night we made efforts to\r\nsecure our position in the crater against the missiles of the\r\nenemy, so as to run trenches along the outer base of their parapet,\r\nright and left; but the enemy continued throwing their grenades,\r\nand brought boxes of field ammunition (shells), the fuses of which\r\nthey would light with portfires, and throw them by hand into our\r\nranks. We found it impossible to continue this work. Another mine\r\nwas consequently started which was exploded on the 1st of July,\r\ndestroying an entire rebel redan, killing and wounding a\r\nconsiderable number of its occupants and leaving an immense chasm\r\nwhere it stood. No attempt to charge was made this time, the\r\nexperience of the 25th admonishing us. Our loss in the first affair\r\nwas about thirty killed and wounded. The enemy must have lost more\r\nin the two explosions than we did in the first. We lost none in the\r\nsecond.', 'Paragraph 540': 'From this time forward the work of mining and pushing our\r\nposition nearer to the enemy was prosecuted with vigor, and I\r\ndetermined to explode no more mines until we were ready to explode\r\na number at different points and assault immediately after. We were\r\nup now at three different points, one in front of each corps, to\r\nwhere only the parapet of the enemy divided us.', 'Paragraph 541': 'At this time an intercepted dispatch from Johnston to Pemberton\r\ninformed me that Johnston intended to make a determined attack upon\r\nus in order to relieve the garrison at Vicksburg. I knew the\r\ngarrison would make no formidable effort to relieve itself. The\r\npicket lines were so close to each other—where there was\r\nspace enough between the lines to post pickets—that the men\r\ncould converse. On the 21st of June I was informed, through this\r\nmeans, that Pemberton was preparing to escape, by crossing to the\r\nLouisiana side under cover of night; that he had employed workmen\r\nin making boats for that purpose; that the men had been canvassed\r\nto ascertain if they would make an assault on the "Yankees" to cut\r\ntheir way out; that they had refused, and almost mutinied, because\r\ntheir commander would not surrender and relieve their sufferings,\r\nand had only been pacified by the assurance that boats enough would\r\nbe finished in a week to carry them all over. The rebel pickets\r\nalso said that houses in the city had been pulled down to get\r\nmaterial to build these boats with. Afterwards this story was\r\nverified: on entering the city we found a large number of very\r\nrudely constructed boats.', 'Paragraph 542': 'All necessary steps were at once taken to render such an attempt\r\nabortive. Our pickets were doubled; Admiral Porter was notified, so\r\nthat the river might be more closely watched; material was\r\ncollected on the west bank of the river to be set on fire and light\r\nup the river if the attempt was made; and batteries were\r\nestablished along the levee crossing the peninsula on the Louisiana\r\nside. Had the attempt been made the garrison of Vicksburg would\r\nhave been drowned, or made prisoners on the Louisiana side. General\r\nRichard Taylor was expected on the west bank to co-operate in this\r\nmovement, I believe, but he did not come, nor could he have done so\r\nwith a force sufficient to be of service. The Mississippi was now\r\nin our possession from its source to its mouth, except in the\r\nimmediate front of Vicksburg and of Port Hudson. We had nearly\r\nexhausted the country, along a line drawn from Lake Providence to\r\nopposite Bruinsburg. The roads west were not of a character to draw\r\nsupplies over for any considerable force.', 'Paragraph 543': "By the 1st of July our approaches had reached the enemy's ditch\r\nat a number of places. At ten points we could move under cover to\r\nwithin from five to one hundred yards of the enemy. Orders were\r\ngiven to make all preparations for assault on the 6th of July. The\r\ndebouches were ordered widened to afford easy egress, while the\r\napproaches were also to be widened to admit the troops to pass\r\nthrough four abreast. Plank, and bags filled with cotton packed in\r\ntightly, were ordered prepared, to enable the troops to cross the\r\nditches.", 'Paragraph 544': 'On the night of the 1st of July Johnston was between Brownsville\r\nand the Big Black, and wrote Pemberton from there that about the\r\n7th of the month an attempt would be made to create a diversion to\r\nenable him to cut his way out. Pemberton was a prisoner before this\r\nmessage reached him.', 'Paragraph 545': 'On July 1st Pemberton, seeing no hope of outside relief,\r\naddressed the following letter to each of his four division\r\ncommanders:', 'Paragraph 546': '"Unless the siege of Vicksburg is raised, or supplies are thrown\r\nin, it will become necessary very shortly to evacuate the place. I\r\nsee no prospect of the former, and there are many great, if not\r\ninsuperable obstacles in the way of the latter. You are, therefore,\r\nrequested to inform me with as little delay as possible, as to the\r\ncondition of your troops and their ability to make the marches and\r\nundergo the fatigues necessary to accomplish a successful\r\nevacuation."', 'Paragraph 547': 'Two of his generals suggested surrender, and the other two\r\npractically did the same. They expressed the opinion that an\r\nattempt to evacuate would fail. Pemberton had previously got a\r\nmessage to Johnston suggesting that he should try to negotiate with\r\nme for a release of the garrison with their arms. Johnston replied\r\nthat it would be a confession of weakness for him to do so; but he\r\nauthorized Pemberton to use his name in making such an\r\narrangement.', 'Paragraph 548': "On the 3d about ten o'clock A.M. white flags appeared on a\r\nportion of the rebel works. Hostilities along that part of the line\r\nceased at once. Soon two persons were seen coming towards our lines\r\nbearing a white flag. They proved to be General Bowen, a division\r\ncommander, and Colonel Montgomery, aide-de-camp to Pemberton,\r\nbearing the following letter to me:", 'Paragraph 549': '"I have the honor to propose an armistice for—hours, with\r\nthe view to arranging terms for the capitulation of Vicksburg. To\r\nthis end, if agreeable to you, I will appoint three commissioners,\r\nto meet a like number to be named by yourself at such place and\r\nhour to-day as you may find convenient. I make this proposition to\r\nsave the further effusion of blood, which must otherwise be shed to\r\na frightful extent, feeling myself fully able to maintain my\r\nposition for a yet indefinite period. This communication will be\r\nhanded you under a flag of truce, by Major-General John S.\r\nBowen."', 'Paragraph 550': 'It was a glorious sight to officers and soldiers on the line\r\nwhere these white flags were visible, and the news soon spread to\r\nall parts of the command. The troops felt that their long and weary\r\nmarches, hard fighting, ceaseless watching by night and day, in a\r\nhot climate, exposure to all sorts of weather, to diseases and,\r\nworst of all, to the gibes of many Northern papers that came to\r\nthem saying all their suffering was in vain, that Vicksburg would\r\nnever be taken, were at last at an end and the Union sure to be\r\nsaved.', 'Paragraph 551': "Bowen was received by General A. J. Smith, and asked to see me.\r\nI had been a neighbor of Bowen's in Missouri, and knew him well and\r\nfavorably before the war; but his request was refused. He then\r\nsuggested that I should meet Pemberton. To this I sent a verbal\r\nmessage saying that, if Pemberton desired it, I would meet him in\r\nfront of McPherson's corps at three o'clock that afternoon. I also\r\nsent the following written reply to Pemberton's letter:", 'Paragraph 552': '"Your note of this date is just received, proposing an armistice\r\nfor several hours, for the purpose of arranging terms of\r\ncapitulation through commissioners, to be appointed, etc. The\r\nuseless effusion of blood you propose stopping by this course can\r\nbe ended at any time you may choose, by the unconditional surrender\r\nof the city and garrison. Men who have shown so much endurance and\r\ncourage as those now in Vicksburg, will always challenge the\r\nrespect of an adversary, and I can assure you will be treated with\r\nall the respect due to prisoners of war. I do not favor the\r\nproposition of appointing commissioners to arrange the terms of\r\ncapitulation, because I have no terms other than those indicated\r\nabove."', 'Paragraph 553': 'At three o\'clock Pemberton appeared at the point suggested in my\r\nverbal message, accompanied by the same officers who had borne his\r\nletter of the morning. Generals Ord, McPherson, Logan and A. J.\r\nSmith, and several officers of my staff, accompanied me. Our place\r\nof meeting was on a hillside within a few hundred feet of the rebel\r\nlines. Near by stood a stunted oak-tree, which was made historical\r\nby the event. It was but a short time before the last vestige of\r\nits body, root and limb had disappeared, the fragments taken as\r\ntrophies. Since then the same tree has furnished as many cords of\r\nwood, in the shape of trophies, as "The True Cross."', 'Paragraph 554': 'Pemberton and I had served in the same division during part of\r\nthe Mexican War. I knew him very well therefore, and greeted him as\r\nan old acquaintance. He soon asked what terms I proposed to give\r\nhis army if it surrendered. My answer was the same as proposed in\r\nmy reply to his letter. Pemberton then said, rather snappishly,\r\n"The conference might as well end," and turned abruptly as if to\r\nleave. I said, "Very well." General Bowen, I saw, was very anxious\r\nthat the surrender should be consummated. His manner and remarks\r\nwhile Pemberton and I were talking, showed this. He now proposed\r\nthat he and one of our generals should have a conference. I had no\r\nobjection to this, as nothing could be made binding upon me that\r\nthey might propose. Smith and Bowen accordingly had a conference,\r\nduring which Pemberton and I, moving a short distance away towards\r\nthe enemy\'s lines were in conversation. After a while Bowen\r\nsuggested that the Confederate army should be allowed to march out\r\nwith the honors of war, carrying their small arms and field\r\nartillery. This was promptly and unceremoniously rejected. The\r\ninterview here ended, I agreeing, however, to send a letter giving\r\nfinal terms by ten o\'clock that night.', 'Paragraph 555': 'Word was sent to Admiral Porter soon after the correspondence\r\nwith Pemberton commenced, so that hostilities might be stopped on\r\nthe part of both army and navy. It was agreed on my paging with\r\nPemberton that they should not be renewed until our correspondence\r\nceased.', 'Paragraph 556': 'When I returned to my headquarters I sent for all the corps and\r\ndivision commanders with the army immediately confronting\r\nVicksburg. Half the army was from eight to twelve miles off,\r\nwaiting for Johnston. I informed them of the contents of\r\nPemberton\'s letters, of my reply and the substance of the\r\ninterview, and that I was ready to hear any suggestion; but would\r\nhold the power of deciding entirely in my own hands. This was the\r\nnearest approach to a "council of war" I ever held. Against the\r\ngeneral, and almost unanimous judgment of the council I sent the\r\nfollowing letter:', 'Paragraph 557': '"In conformity with agreement of this afternoon, I will submit\r\nthe following proposition for the surrender of the City of\r\nVicksburg, public stores, etc. On your accepting the terms\r\nproposed, I will march in one division as a guard, and take\r\npossession at eight A.M. to-morrow. As soon as rolls can be made\r\nout, and paroles be signed by officers and men, you will be allowed\r\nto march out of our lines, the officers taking with them their\r\nside-arms and clothing, and the field, staff and cavalry officers\r\none horse each. The rank and file will be allowed all their\r\nclothing, but no other property. If these conditions are accepted,\r\nany amount of rations you may deem necessary can be taken from the\r\nstores you now have, and also the necessary cooking utensils for\r\npreparing them. Thirty wagons also, counting two two-horse or mule\r\nteams as one, will be allowed to transport such articles as cannot\r\nbe carried along. The same conditions will be allowed to all sick\r\nand wounded officers and soldiers as fast as they become able to\r\ntravel. The paroles for these latter must be signed, however,\r\nwhilst officers present are authorized to sign the roll of\r\nprisoners."', 'Paragraph 558': "By the terms of the cartel then in force, prisoners captured by\r\neither army were required to be forwarded as soon as possible to\r\neither Aiken's landing below Dutch Gap on the James River, or to\r\nVicksburg, there to be exchanged, or paroled until they could be\r\nexchanged. There was a Confederate commissioner at Vicksburg,\r\nauthorized to make the exchange. I did not propose to take him a\r\nprisoner, but to leave him free to perform the functions of his\r\noffice. Had I insisted upon an unconditional surrender there would\r\nhave been over thirty thousand men to transport to Cairo, very much\r\nto the inconvenience of the army on the Mississippi. Thence the\r\nprisoners would have had to be transported by rail to Washington or\r\nBaltimore; thence again by steamer to Aiken's—all at very\r\ngreat expense. At Aiken's they would have had to be paroled,\r\nbecause the Confederates did not have Union prisoners to give in\r\nexchange. Then again Pemberton's army was largely composed of men\r\nwhose homes were in the South-west; I knew many of them were tired\r\nof the war and would get home just as soon as they could. A large\r\nnumber of them had voluntarily come into our lines during the\r\nsiege, and requested to be sent north where they could get\r\nemployment until the war was over and they could go to their\r\nhomes.", 'Paragraph 559': 'Late at night I received the following reply to my last\r\nletter:', 'Paragraph 560': '"I have the honor to acknowledge the receipt of your\r\ncommunication of this date, proposing terms of capitulation for\r\nthis garrison and post. In the main your terms are accepted; but,\r\nin justice both to the honor and spirit of my troops manifested in\r\nthe defence of Vicksburg, I have to submit the following\r\namendments, which, if acceded to by you, will perfect the agreement\r\nbetween us. At ten o\'clock A.M. to-morrow, I propose to evacuate\r\nthe works in and around Vicksburg, and to surrender the city and\r\ngarrison under my command, by marching out with my colors and arms,\r\nstacking them in front of my present lines. After which you will\r\ntake possession. Officers to retain their side-arms and personal\r\nproperty, and the rights and property of citizens to be\r\nrespected."', 'Paragraph 561': 'This was received after midnight. My reply was as follows:', 'Paragraph 562': '"I have the honor to acknowledge the receipt of your\r\ncommunication of 3d July. The amendment proposed by you cannot be\r\nacceded to in full. It will be necessary to furnish every officer\r\nand man with a parole signed by himself, which, with the completion\r\nof the roll of prisoners, will necessarily take some time. Again, I\r\ncan make no stipulations with regard to the treatment of citizens\r\nand their private property. While I do not propose to cause them\r\nany undue annoyance or loss, I cannot consent to leave myself under\r\nany restraint by stipulations. The property which officers will be\r\nallowed to take with them will be as stated in my proposition of\r\nlast evening; that is, officers will be allowed their private\r\nbaggage and side-arms, and mounted officers one horse each. If you\r\nmean by your proposition for each brigade to march to the front of\r\nthe lines now occupied by it, and stack arms at ten o\'clock A.M.,\r\nand then return to the inside and there remain as prisoners until\r\nproperly paroled, I will make no objection to it. Should no\r\nnotification be received of your acceptance of my terms by nine\r\no\'clock A.M. I shall regard them as having been rejected, and shall\r\nact accordingly. Should these terms be accepted, white flags should\r\nbe displayed along your lines to prevent such of my troops as may\r\nnot have been notified, from firing upon your men."', 'Paragraph 563': 'Pemberton promptly accepted these terms.', 'Paragraph 564': 'During the siege there had been a good deal of friendly sparring\r\nbetween the soldiers of the two armies, on picket and where the\r\nlines were close together. All rebels were known as "Johnnies," all\r\nUnion troops as "Yanks." Often "Johnny" would call: "Well, Yank,\r\nwhen are you coming into town?" The reply was sometimes: "We\r\npropose to celebrate the 4th of July there." Sometimes it would be:\r\n"We always treat our prisoners with kindness and do not want to\r\nhurt them;" or, "We are holding you as prisoners of war while you\r\nare feeding yourselves." The garrison, from the commanding general\r\ndown, undoubtedly expected an assault on the fourth. They knew from\r\nthe temper of their men it would be successful when made; and that\r\nwould be a greater humiliation than to surrender. Besides it would\r\nbe attended with severe loss to them.', 'Paragraph 565': 'The Vicksburg paper, which we received regularly through the\r\ncourtesy of the rebel pickets, said prior to the fourth, in\r\nspeaking of the "Yankee" boast that they would take dinner in\r\nVicksburg that day, that the best receipt for cooking a rabbit was\r\n"First ketch your rabbit." The paper at this time and for some time\r\nprevious was printed on the plain side of wall paper. The last\r\nnumber was issued on the fourth and announced that we had "caught\r\nour rabbit."', 'Paragraph 566': 'I have no doubt that Pemberton commenced his correspondence on\r\nthe third with a two-fold purpose: first, to avoid an assault,\r\nwhich he knew would be successful, and second, to prevent the\r\ncapture taking place on the great national holiday, the anniversary\r\nof the Declaration of American Independence. Holding out for better\r\nterms as he did he defeated his aim in the latter particular.', 'Paragraph 567': "At the appointed hour the garrison of Vicksburg marched out of\r\ntheir works and formed line in front, stacked arms and marched back\r\nin good order. Our whole army present witnessed this scene without\r\ncheering. Logan's division, which had approached nearest the rebel\r\nworks, was the first to march in; and the flag of one of the\r\nregiments of his division was soon floating over the court-house.\r\nOur soldiers were no sooner inside the lines than the two armies\r\nbegan to fraternize. Our men had had full rations from the time the\r\nsiege commenced, to the close. The enemy had been suffering,\r\nparticularly towards the last. I myself saw our men taking bread\r\nfrom their haversacks and giving it to the enemy they had so\r\nrecently been engaged in starving out. It was accepted with avidity\r\nand with thanks.", 'Paragraph 568': 'Pemberton says in his report:', 'Paragraph 569': '"If it should be asked why the 4th of July was selected as the\r\nday for surrender, the answer is obvious. I believed that upon that\r\nday I should obtain better terms. Well aware of the vanity of our\r\nfoe, I knew they would attach vast importance to the entrance on\r\nthe 4th of July into the stronghold of the great river, and that,\r\nto gratify their national vanity, they would yield then what could\r\nnot be extorted from them at any other time."', 'Paragraph 570': "This does not support my view of his reasons for selecting the\r\nday he did for surrendering. But it must be recollected that his\r\nfirst letter asking terms was received about 10 o'clock A.M., July\r\n3d. It then could hardly be expected that it would take twenty-four\r\nhours to effect a surrender. He knew that Johnston was in our rear\r\nfor the purpose of raising the siege, and he naturally would want\r\nto hold out as long as he could. He knew his men would not resist\r\nan assault, and one was expected on the fourth. In our interview he\r\ntold me he had rations enough to hold out for some time—my\r\nrecollection is two weeks. It was this statement that induced me to\r\ninsert in the terms that he was to draw rations for his men from\r\nhis own supplies.", 'Paragraph 571': 'On the 4th of July General Holmes, with an army of eight or nine\r\nthousand men belonging to the trans-Mississippi department, made an\r\nattack upon Helena, Arkansas. He was totally defeated by General\r\nPrentiss, who was holding Helena with less than forty-two hundred\r\nsoldiers. Holmes reported his loss at 1,636, of which 173 were\r\nkilled; but as Prentiss buried 400, Holmes evidently understated\r\nhis losses. The Union loss was 57 killed, 127 wounded, and between\r\n30 and 40 missing. This was the last effort on the part of the\r\nConfederacy to raise the siege of Vicksburg.', 'Paragraph 572': 'On the third, as soon as negotiations were commenced, I notified\r\nSherman and directed him to be ready to take the offensive against\r\nJohnston, drive him out of the State and destroy his army if he\r\ncould. Steele and Ord were directed at the same time to be in\r\nreadiness to join Sherman as soon as the surrender took place. Of\r\nthis Sherman was notified.', 'Paragraph 573': 'I rode into Vicksburg with the troops, and went to the river to\r\nexchange congratulations with the navy upon our joint victory. At\r\nthat time I found that many of the citizens had been living under\r\nground. The ridges upon which Vicksburg is built, and those back to\r\nthe Big Black, are composed of a deep yellow clay of great\r\ntenacity. Where roads and streets are cut through, perpendicular\r\nbanks are left and stand as well as if composed of stone. The\r\nmagazines of the enemy were made by running passage-ways into this\r\nclay at places where there were deep cuts. Many citizens secured\r\nplaces of safety for their families by carving out rooms in these\r\nembankments. A door-way in these cases would be cut in a high bank,\r\nstarting from the level of the road or street, and after running in\r\na few feet a room of the size required was carved out of the clay,\r\nthe dirt being removed by the door-way. In some instances I saw\r\nwhere two rooms were cut out, for a single family, with a door-way\r\nin the clay wall separating them. Some of these were carpeted and\r\nfurnished with considerable elaboration. In these the occupants\r\nwere fully secure from the shells of the navy, which were dropped\r\ninto the city night and dav without intermission.', 'Paragraph 574': 'I returned to my old headquarters outside in the afternoon, and\r\ndid not move into the town until the sixth. On the afternoon of the\r\nfourth I sent Captain Wm. M. Dunn of my staff to Cairo, the nearest\r\npoint where the telegraph could be reached, with a dispatch to the\r\ngeneral-in-chief. It was as follows:', 'Paragraph 575': '"The enemy surrendered this morning. The only terms allowed is\r\ntheir parole as prisoners of war. This I regard as a great\r\nadvantage to us at this moment. It saves, probably, several days in\r\nthe capture, and leaves troops and transports ready for immediate\r\nservice. Sherman, with a large force, moves immediately on\r\nJohnston, to drive him from the State. I will send troops to the\r\nrelief of Banks, and return the 9th army corps to Burnside."', 'Paragraph 576': 'This news, with the victory at Gettysburg won the same day,\r\nlifted a great load of anxiety from the minds of the President, his\r\nCabinet and the loyal people all over the North. The fate of the\r\nConfederacy was sealed when Vicksburg fell. Much hard fighting was\r\nto be done afterwards and many precious lives were to be\r\nsacrificed; but the MORALE was with the supporters of the Union\r\never after.', 'Paragraph 577': 'I at the same time wrote to General Banks informing him of the\r\nfall and sending him a copy of the terms; also saying I would send\r\nhim all the troops he wanted to insure the capture of the only\r\nfoothold the enemy now had on the Mississippi River. General Banks\r\nhad a number of copies of this letter printed, or at least a\r\nsynopsis of it, and very soon a copy fell into the hands of General\r\nGardner, who was then in command of Port Hudson. Gardner at once\r\nsent a letter to the commander of the National forces saying that\r\nhe had been informed of the surrender of Vicksburg and telling how\r\nthe information reached him. He added that if this was true, it was\r\nuseless for him to hold out longer. General Banks gave him\r\nassurances that Vicksburg had been surrendered, and General Gardner\r\nsurrendered unconditionally on the 9th of July. Port Hudson with\r\nnearly 6,000 prisoners, 51 guns, 5,000 small-arms and other stores\r\nfell into the hands of the Union forces: from that day to the close\r\nof the rebellion the Mississippi River, from its source to its\r\nmouth, remained in the control of the National troops.', 'Paragraph 578': 'Pemberton and his army were kept in Vicksburg until the whole\r\ncould be paroled. The paroles were in duplicate, by organization\r\n(one copy for each, Federals and Confederates), and signed by the\r\ncommanding officers of the companies or regiments. Duplicates were\r\nalso made for each soldier and signed by each individually, one to\r\nbe retained by the soldier signing and one to be retained by us.\r\nSeveral hundred refused to sign their paroles, preferring to be\r\nsent to the North as prisoners to being sent back to fight again.\r\nOthers again kept out of the way, hoping to escape either\r\nalternative.', 'Paragraph 579': 'Pemberton appealed to me in person to compel these men to sign\r\ntheir paroles, but I declined. It also leaked out that many of the\r\nmen who had signed their paroles, intended to desert and go to\r\ntheir homes as soon as they got out of our lines. Pemberton hearing\r\nthis, again appealed to me to assist him. He wanted arms for a\r\nbattalion, to act as guards in keeping his men together while being\r\nmarched to a camp of instruction, where he expected to keep them\r\nuntil exchanged. This request was also declined. It was precisely\r\nwhat I expected and hoped that they would do. I told him, however,\r\nthat I would see that they marched beyond our lines in good order.\r\nBy the eleventh, just one week after the surrender, the paroles\r\nwere completed and the Confederate garrison marched out. Many\r\ndeserted, and fewer of them were ever returned to the ranks to\r\nfight again than would have been the case had the surrender been\r\nunconditional and the prisoners sent to the James River to be\r\nparoled.', 'Paragraph 580': 'As soon as our troops took possession of the city guards were\r\nestablished along the whole line of parapet, from the river above\r\nto the river below. The prisoners were allowed to occupy their old\r\ncamps behind the intrenchments. No restraint was put upon them,\r\nexcept by their own commanders. They were rationed about as our own\r\nmen, and from our supplies. The men of the two armies fraternized\r\nas if they had been fighting for the same cause. When they passed\r\nout of the works they had so long and so gallantly defended,\r\nbetween lines of their late antagonists, not a cheer went up, not a\r\nremark was made that would give pain. Really, I believe there was a\r\nfeeling of sadness just then in the breasts of most of the Union\r\nsoldiers at seeing the dejection of their late antagonists.', 'Paragraph 581': 'The day before the departure the following order was issued:', 'Paragraph 582': '"Paroled prisoners will be sent out of here to-morrow. They will\r\nbe authorized to cross at the railroad bridge, and move from there\r\nto Edward\'s Ferry, [Meant Edward\'s Station.] and on by way of\r\nRaymond. Instruct the commands to be orderly and quiet as these\r\nprisoners pass, to make no offensive remarks, and not to harbor any\r\nwho fall out of ranks after they have passed."', 'Paragraph 583': 'The capture of Vicksburg, with its garrison, ordnance and\r\nordnance stores, and the successful battles fought in reaching\r\nthem, gave new spirit to the loyal people of the North. New hopes\r\nfor the final success of the cause of the Union were inspired. The\r\nvictory gained at Gettysburg, upon the same day, added to their\r\nhopes. Now the Mississippi River was entirely in the possession of\r\nthe National troops; for the fall of Vicksburg gave us Port Hudson\r\nat once. The army of northern Virginia was driven out of\r\nPennsylvania and forced back to about the same ground it occupied\r\nin 1861. The Army of the Tennessee united with the Army of the\r\nGulf, dividing the Confederate States completely.', 'Paragraph 584': 'The first dispatch I received from the government after the fall\r\nof Vicksburg was in these words:', 'Paragraph 585': '"I fear your paroling the prisoners at Vicksburg, without actual\r\ndelivery to a proper agent as required by the seventh article of\r\nthe cartel, may be construed into an absolute release, and that the\r\nmen will immediately be placed in the ranks of the enemy. Such has\r\nbeen the case elsewhere. If these prisoners have not been allowed\r\nto depart, you will detain them until further orders."', 'Paragraph 586': 'Halleck did not know that they had already been delivered into\r\nthe hands of Major Watts, Confederate commissioner for the exchange\r\nof prisoners.', 'Paragraph 587': 'At Vicksburg 31,600 prisoners were surrendered, together with\r\n172 cannon about 60,000 muskets and a large amount of ammunition.\r\nThe small-arms of the enemy were far superior to the bulk of ours.\r\nUp to this time our troops at the West had been limited to the old\r\nUnited States flint-lock muskets changed into percussion, or the\r\nBelgian musket imported early in the war—almost as dangerous\r\nto the person firing it as to the one aimed at—and a few new\r\nand improved arms. These were of many different calibers, a fact\r\nthat caused much trouble in distributing ammunition during an\r\nengagement. The enemy had generally new arms which had run the\r\nblockade and were of uniform caliber. After the surrender I\r\nauthorized all colonels whose regiments were armed with inferior\r\nmuskets, to place them in the stack of captured arms and replace\r\nthem with the latter. A large number of arms turned in to the\r\nOrdnance Department as captured, were thus arms that had really\r\nbeen used by the Union army in the capture of Vicksburg.', 'Paragraph 588': 'In this narrative I have not made the mention I should like of\r\nofficers, dead and alive, whose services entitle them to special\r\nmention. Neither have I made that mention of the navy which its\r\nservices deserve. Suffice it to say, the close of the siege of\r\nVicksburg found us with an army unsurpassed, in proportion to its\r\nnumbers, taken as a whole of officers and men. A military education\r\nwas acquired which no other school could have given. Men who\r\nthought a company was quite enough for them to command properly at\r\nthe beginning, would have made good regimental or brigade\r\ncommanders; most of the brigade commanders were equal to the\r\ncommand of a division, and one, Ransom, would have been equal to\r\nthe command of a corps at least. Logan and Crocker ended the\r\ncampaign fitted to command independent armies.', 'Paragraph 589': "General F. P. Blair joined me at Milliken's Bend a full-fledged\r\ngeneral, without having served in a lower grade. He commanded a\r\ndivision in the campaign. I had known Blair in Missouri, where I\r\nhad voted against him in 1858 when he ran for Congress. I knew him\r\nas a frank, positive and generous man, true to his friends even to\r\na fault, but always a leader. I dreaded his coming; I knew from\r\nexperience that it was more difficult to command two generals\r\ndesiring to be leaders than it was to command one army officered\r\nintelligently and with subordination. It affords me the greatest\r\npleasure to record now my agreeable disappointment in respect to\r\nhis character. There was no man braver than he, nor was there any\r\nwho obeyed all orders of his superior in rank with more\r\nunquestioning alacrity. He was one man as a soldier, another as a\r\npolitician.", 'Paragraph 590': 'The navy under Porter was all it could be, during the entire\r\ncampaign. Without its assistance the campaign could not have been\r\nsuccessfully made with twice the number of men engaged. It could\r\nnot have been made at all, in the way it was, with any number of\r\nmen without such assistance. The most perfect harmony reigned\r\nbetween the two arms of the service. There never was a request\r\nmade, that I am aware of, either of the flag-officer or any of his\r\nsubordinates, that was not promptly complied with.', 'Paragraph 591': "The campaign of Vicksburg was suggested and developed by\r\ncircumstances. The elections of 1862 had gone against the\r\nprosecution of the war. Voluntary enlistments had nearly ceased and\r\nthe draft had been resorted to; this was resisted, and a defeat or\r\nbackward movement would have made its execution impossible. A\r\nforward movement to a decisive victory was necessary. Accordingly I\r\nresolved to get below Vicksburg, unite with Banks against Port\r\nHudson, make New Orleans a base and, with that base and Grand Gulf\r\nas a starting point, move our combined forces against Vicksburg.\r\nUpon reaching Grand Gulf, after running its batteries and fighting\r\na battle, I received a letter from Banks informing me that he could\r\nnot be at Port Hudson under ten days, and then with only fifteen\r\nthousand men. The time was worth more than the reinforcements; I\r\ntherefore determined to push into the interior of the enemy's\r\ncountry.", 'Paragraph 592': 'With a large river behind us, held above and below by the enemy,\r\nrapid movements were essential to success. Jackson was captured the\r\nday after a new commander had arrived, and only a few days before\r\nlarge reinforcements were expected. A rapid movement west was made;\r\nthe garrison of Vicksburg was met in two engagements and badly\r\ndefeated, and driven back into its stronghold and there\r\nsuccessfully besieged. It looks now as though Providence had\r\ndirected the course of the campaign while the Army of the Tennessee\r\nexecuted the decree.', 'Paragraph 593': 'Upon the surrender of the garrison of Vicksburg there were three\r\nthings that required immediate attention. The first was to send a\r\nforce to drive the enemy from our rear, and out of the State. The\r\nsecond was to send reinforcements to Banks near Port Hudson, if\r\nnecessary, to complete the triumph of opening the Mississippi from\r\nits source to its mouth to the free navigation of vessels bearing\r\nthe Stars and Stripes. The third was to inform the authorities at\r\nWashington and the North of the good news, to relieve their long\r\nsuspense and strengthen their confidence in the ultimate success of\r\nthe cause they had so much at heart.', 'Paragraph 594': "Soon after negotiations were opened with General Pemberton for\r\nthe surrender of the city, I notified Sherman, whose troops\r\nextended from Haines' Bluff on the left to the crossing of the\r\nVicksburg and Jackson road over the Big Black on the right, and\r\ndirected him to hold his command in readiness to advance and drive\r\nthe enemy from the State as soon as Vicksburg surrendered. Steele\r\nand Ord were directed to be in readiness to join Sherman in his\r\nmove against General Johnston, and Sherman was advised of this\r\nalso. Sherman moved promptly, crossing the Big Black at three\r\ndifferent points with as many columns, all concentrating at Bolton,\r\ntwenty miles west of Jackson.", 'Paragraph 595': "Johnston heard of the surrender of Vicksburg almost as soon as\r\nit occurred, and immediately fell back on Jackson. On the 8th of\r\nJuly Sherman was within ten miles of Jackson and on the 11th was\r\nclose up to the defences of the city and shelling the town. The\r\nsiege was kept up until the morning of the 17th, when it was found\r\nthat the enemy had evacuated during the night. The weather was very\r\nhot, the roads dusty and the water bad. Johnston destroyed the\r\nroads as he passed and had so much the start that pursuit was\r\nuseless; but Sherman sent one division, Steele's, to Brandon,\r\nfourteen miles east of Jackson.", 'Paragraph 596': 'The National loss in the second capture of Jackson was less than\r\none thousand men, killed, wounded and missing. The Confederate loss\r\nwas probably less, except in captured. More than this number fell\r\ninto our hands as prisoners.', 'Paragraph 597': 'Medicines and food were left for the Confederate wounded and\r\nsick who had to be left behind. A large amount of rations was\r\nissued to the families that remained in Jackson. Medicine and food\r\nwere also sent to Raymond for the destitute families as well as the\r\nsick and wounded, as I thought it only fair that we should return\r\nto these people some of the articles we had taken while marching\r\nthrough the country. I wrote to Sherman: "Impress upon the men the\r\nimportance of going through the State in an orderly manner,\r\nabstaining from taking anything not absolutely necessary for their\r\nsubsistence while travelling. They should try to create as\r\nfavorable an impression as possible upon the people." Provisions\r\nand forage, when called for by them, were issued to all the people,\r\nfrom Bruinsburg to Jackson and back to Vicksburg, whose resources\r\nhad been taken for the supply of our army. Very large quantities of\r\ngroceries and provisions were so issued.', 'Paragraph 598': "Sherman was ordered back to Vicksburg, and his troops took much\r\nthe same position they had occupied before—from the Big Black\r\nto Haines' Bluff. Having cleaned up about Vicksburg and captured or\r\nrouted all regular Confederate forces for more than a hundred miles\r\nin all directions, I felt that the troops that had done so much\r\nshould be allowed to do more before the enemy could recover from\r\nthe blow he had received, and while important points might be\r\ncaptured without bloodshed. I suggested to the General-in-chief the\r\nidea of a campaign against Mobile, starting from Lake\r\nPontchartrain. Halleck preferred another course. The possession of\r\nthe trans-Mississippi by the Union forces seemed to possess more\r\nimportance in his mind than almost any campaign east of the\r\nMississippi. I am well aware that the President was very anxious to\r\nhave a foothold in Texas, to stop the clamor of some of the foreign\r\ngovernments which seemed to be seeking a pretext to interfere in\r\nthe war, at least so far as to recognize belligerent rights to the\r\nConfederate States. This, however, could have been easily done\r\nwithout wasting troops in western Louisiana and eastern Texas, by\r\nsending a garrison at once to Brownsville on the Rio Grande.", 'Paragraph 599': "Halleck disapproved of my proposition to go against Mobile, so\r\nthat I was obliged to settle down and see myself put again on the\r\ndefensive as I had been a year before in west Tennessee. It would\r\nhave been an easy thing to capture Mobile at the time I proposed to\r\ngo there. Having that as a base of operations, troops could have\r\nbeen thrown into the interior to operate against General Bragg's\r\narmy. This would necessarily have compelled Bragg to detach in\r\norder to meet this fire in his rear. If he had not done this the\r\ntroops from Mobile could have inflicted inestimable damage upon\r\nmuch of the country from which his army and Lee's were yet\r\nreceiving their supplies. I was so much impressed with this idea\r\nthat I renewed my request later in July and again about the 1st of\r\nAugust, and proposed sending all the troops necessary, asking only\r\nthe assistance of the navy to protect the debarkation of troops at\r\nor near Mobile. I also asked for a leave of absence to visit New\r\nOrleans, particularly if my suggestion to move against Mobile\r\nshould be approved. Both requests were refused. So far as my\r\nexperience with General Halleck went it was very much easier for\r\nhim to refuse a favor than to grant one. But I did not regard this\r\nas a favor. It was simply in line of duty, though out of my\r\ndepartment.", 'Paragraph 600': "The General-in-chief having decided against me, the depletion of\r\nan army, which had won a succession of great victories, commenced,\r\nas had been the case the year before after the fall of Corinth when\r\nthe army was sent where it would do the least good. By orders, I\r\nsent to Banks a force of 4,000 men; returned the 9th corps to\r\nKentucky and, when transportation had been collected, started a\r\ndivision of 5,000 men to Schofield in Missouri where Price was\r\nraiding the State. I also detached a brigade under Ransom to\r\nNatchez, to garrison that place permanently. This latter move was\r\nquite fortunate as to the time when Ransom arrived there. The enemy\r\nhappened to have a large number, about 5,000 head, of beef cattle\r\nthere on the way from Texas to feed the Eastern armies, and also a\r\nlarge amount of munitions of war which had probably come through\r\nTexas from the Rio Grande and which were on the way to Lee's and\r\nother armies in the East.", 'Paragraph 601': 'The troops that were left with me around Vicksburg were very\r\nbusily and unpleasantly employed in making expeditions against\r\nguerilla bands and small detachments of cavalry which infested the\r\ninterior, and in destroying mills, bridges and rolling stock on the\r\nrailroads. The guerillas and cavalry were not there to fight but to\r\nannoy, and therefore disappeared on the first approach of our\r\ntroops.', 'Paragraph 602': "The country back of Vicksburg was filled with deserters from\r\nPemberton's army and, it was reported, many from Johnston's also.\r\nThe men determined not to fight again while the war lasted. Those\r\nwho lived beyond the reach of the Confederate army wanted to get to\r\ntheir homes. Those who did not, wanted to get North where they\r\ncould work for their support till the war was over. Besides all\r\nthis there was quite a peace feeling, for the time being, among the\r\ncitizens of that part of Mississippi, but this feeling soon\r\nsubsided. It is not probable that Pemberton got off with over 4,000\r\nof his army to the camp where he proposed taking them, and these\r\nwere in a demoralized condition.", 'Paragraph 603': 'On the 7th of August I further depleted my army by sending the\r\n13th corps, General Ord commanding, to Banks. Besides this I\r\nreceived orders to co-operate with the latter general in movements\r\nwest of the Mississippi. Having received this order I went to New\r\nOrleans to confer with Banks about the proposed movement. All these\r\nmovements came to naught.', 'Paragraph 604': "During this visit I reviewed Banks' army a short distance above\r\nCarrollton. The horse I rode was vicious and but little used, and\r\non my return to New Orleans ran away and, shying at a locomotive in\r\nthe street, fell, probably on me. I was rendered insensible, and\r\nwhen I regained consciousness I found myself in a hotel near by\r\nwith several doctors attending me. My leg was swollen from the knee\r\nto the thigh, and the swelling, almost to the point of bursting,\r\nextended along the body up to the arm-pit. The pain was almost\r\nbeyond endurance. I lay at the hotel something over a week without\r\nbeing able to turn myself in bed. I had a steamer stop at the\r\nnearest point possible, and was carried to it on a litter. I was\r\nthen taken to Vicksburg, where I remained unable to move for some\r\ntime afterwards.", 'Paragraph 605': 'While I was absent General Sherman declined to assume command\r\nbecause, he said, it would confuse the records; but he let all the\r\norders be made in my name, and was glad to render any assistance he\r\ncould. No orders were issued by my staff, certainly no important\r\norders, except upon consultation with and approval of Sherman.', 'Paragraph 606': "On the 13th of September, while I was still in New Orleans,\r\nHalleck telegraphed to me to send all available forces to Memphis\r\nand thence to Tuscumbia, to co-operate with Rosecrans for the\r\nrelief of Chattanooga. On the 15th he telegraphed again for all\r\navailable forces to go to Rosecrans. This was received on the 27th.\r\nI was still confined to my bed, unable to rise from it without\r\nassistance; but I at once ordered Sherman to send one division to\r\nMemphis as fast as transports could be provided. The division of\r\nMcPherson's corps, which had got off and was on the way to join\r\nSteele in Arkansas, was recalled and sent, likewise, to report to\r\nHurlbut at Memphis. Hurlbut was directed to forward these two\r\ndivisions with two others from his own corps at once, and also to\r\nsend any other troops that might be returning there. Halleck\r\nsuggested that some good man, like Sherman or McPherson, should be\r\nsent to Memphis to take charge of the troops going east. On this I\r\nsent Sherman, as being, I thought, the most suitable person for an\r\nindependent command, and besides he was entitled to it if it had to\r\nbe given to any one. He was directed to take with him another\r\ndivision of his corps. This left one back, but having one of\r\nMcPherson's divisions he had still the equivalent.", 'Paragraph 607': "Before the receipt by me of these orders the battle of\r\nChickamauga had been fought and Rosecrans forced back into\r\nChattanooga. The administration as well as the General-in-chief was\r\nnearly frantic at the situation of affairs there. Mr. Charles A.\r\nDana, an officer of the War Department, was sent to Rosecrans'\r\nheadquarters. I do not know what his instructions were, but he was\r\nstill in Chattanooga when I arrived there at a later period.", 'Paragraph 608': 'It seems that Halleck suggested that I should go to Nashville as\r\nsoon as able to move and take general direction of the troops\r\nmoving from the west. I received the following dispatch dated\r\nOctober 3d: "It is the wish of the Secretary of War that as soon as\r\nGeneral Grant is able he will come to Cairo and report by\r\ntelegraph." I was still very lame, but started without delay.\r\nArriving at Columbus on the 16th I reported by telegraph: "Your\r\ndispatch from Cairo of the 3d directing me to report from Cairo was\r\nreceived at 11.30 on the 10th. Left the same day with staff and\r\nheadquarters and am here en route for Cairo."'}



```python
# creation of dataframe based on individual paragraphs from text corpus
grant = pd.DataFrame.from_dict(paragraph_dict, orient='index').reset_index().rename(columns={'index':'Paragraph',0:'Text'})
```


```python
# testing efficacy of dataframe
grant
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Paragraph</th>
      <th>Text</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Paragraph 1</td>
      <td>My family, all this while, was at the East. It...</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Paragraph 2</td>
      <td>In the late summer of 1854 I rejoined my famil...</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Paragraph 3</td>
      <td>In the winter I established a partnership with...</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Paragraph 4</td>
      <td>While a citizen of Missouri, my first opportun...</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Paragraph 5</td>
      <td>I have no apologies to make for having been on...</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>603</th>
      <td>Paragraph 604</td>
      <td>During this visit I reviewed Banks' army a sho...</td>
    </tr>
    <tr>
      <th>604</th>
      <td>Paragraph 605</td>
      <td>While I was absent General Sherman declined to...</td>
    </tr>
    <tr>
      <th>605</th>
      <td>Paragraph 606</td>
      <td>On the 13th of September, while I was still in...</td>
    </tr>
    <tr>
      <th>606</th>
      <td>Paragraph 607</td>
      <td>Before the receipt by me of these orders the b...</td>
    </tr>
    <tr>
      <th>607</th>
      <td>Paragraph 608</td>
      <td>It seems that Halleck suggested that I should ...</td>
    </tr>
  </tbody>
</table>
<p>608 rows × 2 columns</p>
</div>




```python
# export of dataframe in the form of csv file
grant.to_csv('grant_paragraphs.csv')
```

# Analysis of Dataset


```python
# downloading spaCy's en_core_web_sm small model
!python -m spacy download en_core_web_sm
```

    Collecting en-core-web-sm==3.4.1
      Downloading https://github.com/explosion/spacy-models/releases/download/en_core_web_sm-3.4.1/en_core_web_sm-3.4.1-py3-none-any.whl (12.8 MB)
    [K     |████████████████████████████████| 12.8 MB 4.2 MB/s eta 0:00:01
    [?25hRequirement already satisfied: spacy<3.5.0,>=3.4.0 in /Users/kevin/opt/anaconda3/lib/python3.9/site-packages (from en-core-web-sm==3.4.1) (3.4.1)
    Requirement already satisfied: pydantic!=1.8,!=1.8.1,<1.10.0,>=1.7.4 in /Users/kevin/opt/anaconda3/lib/python3.9/site-packages (from spacy<3.5.0,>=3.4.0->en-core-web-sm==3.4.1) (1.9.2)
    Requirement already satisfied: packaging>=20.0 in /Users/kevin/opt/anaconda3/lib/python3.9/site-packages (from spacy<3.5.0,>=3.4.0->en-core-web-sm==3.4.1) (21.3)
    Requirement already satisfied: tqdm<5.0.0,>=4.38.0 in /Users/kevin/opt/anaconda3/lib/python3.9/site-packages (from spacy<3.5.0,>=3.4.0->en-core-web-sm==3.4.1) (4.64.0)
    Requirement already satisfied: cymem<2.1.0,>=2.0.2 in /Users/kevin/opt/anaconda3/lib/python3.9/site-packages (from spacy<3.5.0,>=3.4.0->en-core-web-sm==3.4.1) (2.0.6)
    Requirement already satisfied: srsly<3.0.0,>=2.4.3 in /Users/kevin/opt/anaconda3/lib/python3.9/site-packages (from spacy<3.5.0,>=3.4.0->en-core-web-sm==3.4.1) (2.4.4)
    Requirement already satisfied: catalogue<2.1.0,>=2.0.6 in /Users/kevin/opt/anaconda3/lib/python3.9/site-packages (from spacy<3.5.0,>=3.4.0->en-core-web-sm==3.4.1) (2.0.8)
    Requirement already satisfied: pathy>=0.3.5 in /Users/kevin/opt/anaconda3/lib/python3.9/site-packages (from spacy<3.5.0,>=3.4.0->en-core-web-sm==3.4.1) (0.6.2)
    Requirement already satisfied: jinja2 in /Users/kevin/opt/anaconda3/lib/python3.9/site-packages (from spacy<3.5.0,>=3.4.0->en-core-web-sm==3.4.1) (2.11.3)
    Requirement already satisfied: numpy>=1.15.0 in /Users/kevin/opt/anaconda3/lib/python3.9/site-packages (from spacy<3.5.0,>=3.4.0->en-core-web-sm==3.4.1) (1.21.5)
    Requirement already satisfied: spacy-legacy<3.1.0,>=3.0.9 in /Users/kevin/opt/anaconda3/lib/python3.9/site-packages (from spacy<3.5.0,>=3.4.0->en-core-web-sm==3.4.1) (3.0.10)
    Requirement already satisfied: setuptools in /Users/kevin/opt/anaconda3/lib/python3.9/site-packages (from spacy<3.5.0,>=3.4.0->en-core-web-sm==3.4.1) (61.2.0)
    Requirement already satisfied: thinc<8.2.0,>=8.1.0 in /Users/kevin/opt/anaconda3/lib/python3.9/site-packages (from spacy<3.5.0,>=3.4.0->en-core-web-sm==3.4.1) (8.1.3)
    Requirement already satisfied: wasabi<1.1.0,>=0.9.1 in /Users/kevin/opt/anaconda3/lib/python3.9/site-packages (from spacy<3.5.0,>=3.4.0->en-core-web-sm==3.4.1) (0.10.1)
    Requirement already satisfied: spacy-loggers<2.0.0,>=1.0.0 in /Users/kevin/opt/anaconda3/lib/python3.9/site-packages (from spacy<3.5.0,>=3.4.0->en-core-web-sm==3.4.1) (1.0.3)
    Requirement already satisfied: typer<0.5.0,>=0.3.0 in /Users/kevin/opt/anaconda3/lib/python3.9/site-packages (from spacy<3.5.0,>=3.4.0->en-core-web-sm==3.4.1) (0.4.2)
    Requirement already satisfied: murmurhash<1.1.0,>=0.28.0 in /Users/kevin/opt/anaconda3/lib/python3.9/site-packages (from spacy<3.5.0,>=3.4.0->en-core-web-sm==3.4.1) (1.0.8)
    Requirement already satisfied: langcodes<4.0.0,>=3.2.0 in /Users/kevin/opt/anaconda3/lib/python3.9/site-packages (from spacy<3.5.0,>=3.4.0->en-core-web-sm==3.4.1) (3.3.0)
    Requirement already satisfied: requests<3.0.0,>=2.13.0 in /Users/kevin/opt/anaconda3/lib/python3.9/site-packages (from spacy<3.5.0,>=3.4.0->en-core-web-sm==3.4.1) (2.27.1)
    Requirement already satisfied: preshed<3.1.0,>=3.0.2 in /Users/kevin/opt/anaconda3/lib/python3.9/site-packages (from spacy<3.5.0,>=3.4.0->en-core-web-sm==3.4.1) (3.0.7)
    Requirement already satisfied: pyparsing!=3.0.5,>=2.0.2 in /Users/kevin/opt/anaconda3/lib/python3.9/site-packages (from packaging>=20.0->spacy<3.5.0,>=3.4.0->en-core-web-sm==3.4.1) (3.0.4)
    Requirement already satisfied: smart-open<6.0.0,>=5.2.1 in /Users/kevin/opt/anaconda3/lib/python3.9/site-packages (from pathy>=0.3.5->spacy<3.5.0,>=3.4.0->en-core-web-sm==3.4.1) (5.2.1)
    Requirement already satisfied: typing-extensions>=3.7.4.3 in /Users/kevin/opt/anaconda3/lib/python3.9/site-packages (from pydantic!=1.8,!=1.8.1,<1.10.0,>=1.7.4->spacy<3.5.0,>=3.4.0->en-core-web-sm==3.4.1) (4.1.1)
    Requirement already satisfied: idna<4,>=2.5 in /Users/kevin/opt/anaconda3/lib/python3.9/site-packages (from requests<3.0.0,>=2.13.0->spacy<3.5.0,>=3.4.0->en-core-web-sm==3.4.1) (3.3)
    Requirement already satisfied: certifi>=2017.4.17 in /Users/kevin/opt/anaconda3/lib/python3.9/site-packages (from requests<3.0.0,>=2.13.0->spacy<3.5.0,>=3.4.0->en-core-web-sm==3.4.1) (2021.10.8)
    Requirement already satisfied: charset-normalizer~=2.0.0 in /Users/kevin/opt/anaconda3/lib/python3.9/site-packages (from requests<3.0.0,>=2.13.0->spacy<3.5.0,>=3.4.0->en-core-web-sm==3.4.1) (2.0.4)
    Requirement already satisfied: urllib3<1.27,>=1.21.1 in /Users/kevin/opt/anaconda3/lib/python3.9/site-packages (from requests<3.0.0,>=2.13.0->spacy<3.5.0,>=3.4.0->en-core-web-sm==3.4.1) (1.26.9)
    Requirement already satisfied: confection<1.0.0,>=0.0.1 in /Users/kevin/opt/anaconda3/lib/python3.9/site-packages (from thinc<8.2.0,>=8.1.0->spacy<3.5.0,>=3.4.0->en-core-web-sm==3.4.1) (0.0.3)
    Requirement already satisfied: blis<0.8.0,>=0.7.8 in /Users/kevin/opt/anaconda3/lib/python3.9/site-packages (from thinc<8.2.0,>=8.1.0->spacy<3.5.0,>=3.4.0->en-core-web-sm==3.4.1) (0.7.8)
    Requirement already satisfied: click<9.0.0,>=7.1.1 in /Users/kevin/opt/anaconda3/lib/python3.9/site-packages (from typer<0.5.0,>=0.3.0->spacy<3.5.0,>=3.4.0->en-core-web-sm==3.4.1) (8.0.4)
    Requirement already satisfied: MarkupSafe>=0.23 in /Users/kevin/opt/anaconda3/lib/python3.9/site-packages (from jinja2->spacy<3.5.0,>=3.4.0->en-core-web-sm==3.4.1) (2.0.1)
    [38;5;2m✔ Download and installation successful[0m
    You can now load the package via spacy.load('en_core_web_sm')



```python
# importing and loading spaCy's en_core_web_sm small model
import spacy
nlp = spacy.load('en_core_web_sm')
```


```python
# initialization of dictionary
place_freq = collections.defaultdict(int)

# filling dictionary with their corresponding frequency values associated with identified GPE or LOCs
for i in range(len(grant)):
    of_interest = grant['Text'][i]
    doc = nlp(of_interest) 
    for entity in doc.ents:
        print(entity)
        if ((entity.label_ == 'GPE') or (entity.label_ == 'LOC')):
            place_freq[entity.text] += 1
            print(type(entity.text))
    
place_freq = dict(place_freq)
```

    East
    <class 'str'>
    two
    Pacific
    <class 'str'>
    March
    the end of the July
    Pacific
    <class 'str'>
    Congress
    the winter of 1863-4
    West
    <class 'str'>
    the late summer of 1854
    Panama
    <class 'str'>
    the age of thirty-two
    St. Louis
    <class 'str'>
    1858
    Ohio
    <class 'str'>
    the fall of 1858
    Harry Boggs
    Grant
    St. Louis
    <class 'str'>
    the spring
    two
    St. Louis
    <class 'str'>
    five
    Boggs
    <class 'str'>
    May, 1860
    Galena
    Illinois
    <class 'str'>
    Missouri
    <class 'str'>
    first
    Presidential
    Clay
    Whig
    the Republican party
    Slave States
    <class 'str'>
    Free States
    <class 'str'>
    St. Louis City
    <class 'str'>
    the Republican party
    the Honorable Frank P. Blair
    Know-Nothings
    American
    just one week later
    one week
    American
    United States
    <class 'str'>
    first
    one
    State
    two
    Mexican
    the United States
    <class 'str'>
    North
    <class 'str'>
    the Democratic party
    Whigs
    Mexican
    Texas
    <class 'str'>
    1856
    first
    The Republican party
    South
    <class 'str'>
    Republican
    1856
    the Slave States
    <class 'str'>
    Democrat
    the Slave States
    <class 'str'>
    four years
    James Buchanan
    Four years
    the Republican party
    Four millions
    Galena
    Galena
    two
    three
    September, 1861
    September, 1861
    the eleven months
    Galena
    first
    November, 1860
    Illinois
    <class 'str'>
    Stephen A. Douglas
    Breckinridge
    Lincoln
    Lincoln
    Galena
    Chicago
    <class 'str'>
    Republican
    the Southern States
    <class 'str'>
    the four years
    first
    Southerners
    Republican
    North-west
    <class 'str'>
    North
    <class 'str'>
    South
    <class 'str'>
    Mormonism and Polygamy
    Southern States
    <class 'str'>
    South
    <class 'str'>
    State
    States
    <class 'str'>
    thirteen
    Constitution
    one
    State
    new States
    <class 'str'>
    States
    <class 'str'>
    Florida
    <class 'str'>
    the States
    <class 'str'>
    Mississippi
    <class 'str'>
    Texas
    <class 'str'>
    Texas
    <class 'str'>
    European
    Russia
    <class 'str'>
    Texas
    <class 'str'>
    South
    <class 'str'>
    States
    <class 'str'>
    South
    <class 'str'>
    Northern
    North
    <class 'str'>
    North
    <class 'str'>
    Southerners
    1861 to 1865
    State
    the latest days
    one
    that day
    Devil
    first
    the winter
    
    1860-1
    south-west
    Wisconsin
    <class 'str'>
    Minnesota
    <class 'str'>
    north-east Iowa
    <class 'str'>
    Mexican
    night
    a late hour
    Seward
    a later day
    ninety days
    Shiloh
    West
    <class 'str'>
    Fort Donelson
    <class 'str'>
    South
    <class 'str'>
    1860
    1861
    one
    North
    <class 'str'>
    South
    <class 'str'>
    Southern
    Northerners
    one
    Southern
    five
    Northern
    South
    <class 'str'>
    North
    <class 'str'>
    Jefferson Davis
    La Grange
    <class 'str'>
    Mississippi
    <class 'str'>
    State
    Mason
    Dixon
    North
    <class 'str'>
    South
    <class 'str'>
    North
    <class 'str'>
    days
    night
    one
    two
    States
    <class 'str'>
    Southern
    Southern
    The States of Virginia
    <class 'str'>
    Kentucky
    <class 'str'>
    one
    State
    The winter of 1860-1
    South Carolina
    <class 'str'>
    Presidential
    Union
    Maryland
    <class 'str'>
    Delaware
    <class 'str'>
    Kentucky
    <class 'str'>
    Missouri
    <class 'str'>
    Slave States
    <class 'str'>
    congress
    Confederate States
    <class 'str'>
    Missouri
    <class 'str'>
    1861
    Jackson
    Reynolds
    State
    the Confederate Government
    South
    <class 'str'>
    States
    <class 'str'>
    States
    <class 'str'>
    Southern
    first
    Buchanan
    Buchanan
    two
    Davis
    Southern
    One
    Floyd
    War
    Northern
    South
    <class 'str'>
    South
    <class 'str'>
    Jefferson Davis
    Montgomery
    <class 'str'>
    Alabama
    <class 'str'>
    Capital
    Loyal
    1860-1
    Southerners
    the
    Union
    <class 'str'>
    North
    <class 'str'>
    South
    <class 'str'>
    North
    <class 'str'>
    North
    <class 'str'>
    4th
    1861
    Abraham Lincoln
    Union
    one
    State
    eleven
    the
    11th
    Fort Sumter
    <class 'str'>
    National
    Charleston
    <class 'str'>
    South Carolina
    <class 'str'>
    Southerners
    a
    few days
    Confederates
    the Constitution of the United States
    Sumter
    Lincoln
    first
    Congress
    75,000
    ninety days'
    Fort Sumter
    75,000
    the Northern States
    <class 'str'>
    a million
    Galena
    evening
    two
    B. B.
    Howard
    Breckinridge
    Democrat
    November
    the fall before
    John A. Rawlins
    Douglas
    E. B. Washburne
    Galena
    Illinois
    <class 'str'>
    six
    one
    Galena
    Galena
    first
    the United States
    <class 'str'>
    a few days
    State
    the morning
    Springfield
    <class 'str'>
    Richard Yates
    ten
    one
    one month
    State
    the United States
    <class 'str'>
    Galena
    the United States
    <class 'str'>
    11th
    Illinois
    <class 'str'>
    Springfield
    <class 'str'>
    nine o'clock
    evening
    Executive
    the next morning
    the State of Illinois
    Loomis
    ten
    State
    three
    State
    One
    Belleville
    <class 'str'>
    eighteen miles
    St. Louis
    <class 'str'>
    only one
    two
    under five days
    a few idle days
    St.
    Louis
    <class 'str'>
    State
    Camp Jackson
    St. Louis
    <class 'str'>
    Claiborn Jackson
    the United States
    <class 'str'>
    St. Louis
    <class 'str'>
    two
    N. Lyon
    Hon
    F. P. Blair
    St. Louis
    <class 'str'>
    Blair
    St. Louis
    <class 'str'>
    1861
    Missouri
    <class 'str'>
    United States
    <class 'str'>
    Blair
    Missouri
    <class 'str'>
    the United States
    <class 'str'>
    Lyon
    Lyon
    Camp Jackson
    two years
    West Point
    <class 'str'>
    Blair
    1858
    Blair
    first
    Honorable
    Major-General F. P. Blair
    Jackson
    St. Louis
    <class 'str'>
    Union
    Pine Street
    Fifth
    Union
    Camp Jackson
    Union
    Pine Street
    St. Louis
    <class 'str'>
    the morning
    4th
    Pine
    this day
    the Union
    only one
    first
    St. Louis
    <class 'str'>
    Yankee
    Camp Jackson
    <class 'str'>
    St. Louis
    <class 'str'>
    The next day
    St. Louis for Mattoon
    <class 'str'>
    Illinois
    <class 'str'>
    the 21st
    Illinois
    <class 'str'>
    one
    State
    John Pope
    Springfield
    <class 'str'>
    United States
    <class 'str'>
    State
    Illinois
    <class 'str'>
    State
    Springfield
    <class 'str'>
    Congress
    State
    State
    S. A. Douglas
    Congress
    Washburne
    Philip Foulk
    first
    Galena
    St. Louis
    <class 'str'>
    three years
    West Point
    <class 'str'>
    Pope
    Mexican
    Taylor
    State
    one
    the United States
    <class 'str'>
    State
    two
    Pope
    Galena
    Army
    GALENA
    ILLINOIS
    <class 'str'>
    May 24, 1861
    COL
    L. THOMAS Adjt
    U. S. A.
    Washington
    <class 'str'>
    D.
    fifteen years
    four years
    West Point
    <class 'str'>
    first
    State
    State
    Springfield
    <class 'str'>
    Illinois
    <class 'str'>
    U. S. GRANT.
    Army
    Badeau
    the
    War Department
    Badeau
    Adjutant-General of
    Army
    Illinois
    <class 'str'>
    Indiana
    <class 'str'>
    State
    a week
    Covington
    <class 'str'>
    Kentucky
    <class 'str'>
    Cincinnati
    <class 'str'>
    McClellan
    Cincinnati
    <class 'str'>
    West Point
    <class 'str'>
    one year
    Mexican
    two successive days
    Springfield
    <class 'str'>
    State
    second
    300,000
    three years
    United States
    <class 'str'>
    State
    two
    Chicago
    <class 'str'>
    19th
    21st
    Mattoon
    <class 'str'>
    A few days
    Springfield
    <class 'str'>
    State
    a few days
    ten
    State
    thirty days
    National
    ninety days'
    three years
    21st
    State
    the United States
    <class 'str'>
    two
    Congress
    State
    McClernand
    Logan
    Logan
    democratic
    Congress
    Logan
    State
    eighteen
    thousand
    Republican
    the Southern States
    <class 'str'>
    South
    <class 'str'>
    first
    Southern
    night
    Union
    National
    Illinois
    <class 'str'>
    Kentucky
    <class 'str'>
    Logan
    Christian
    Republican
    Logan
    Congress
    the
    Union
    <class 'str'>
    first
    Logan
    McClernand
    Republican
    two
    Congress
    a few days
    the United States
    <class 'str'>
    three years
    Logan
    McClernand
    the day
    McClernand
    first
    Logan
    the United States
    <class 'str'>
    Logan
    State
    first
    Illinois
    <class 'str'>
    Logan
    first
    the War Department
    Springfield
    <class 'str'>
    3d
    July
    Quincy
    <class 'str'>
    Illinois
    <class 'str'>
    Springfield
    <class 'str'>
    Quincy
    <class 'str'>
    3d
    July
    the Illinois
    River
    <class 'str'>
    Ironton
    Missouri
    <class 'str'>
    the Illinois River
    <class 'str'>
    St. Louis
    <class 'str'>
    a few miles
    several days
    Illinois
    <class 'str'>
    Hannibal
    <class 'str'>
    St. Joe
    <class 'str'>
    Palmyra
    Missouri
    <class 'str'>
    Quincy
    a few hours
    Galena
    the 21st
    Frederick D. Grant
    eleven years of age
    Quincy
    Grant
    Fred
    Quincy
    <class 'str'>
    Mississippi
    <class 'str'>
    Dubuque
    <class 'str'>
    Iowa
    <class 'str'>
    Galena
    Mexico
    <class 'str'>
    one
    the Mississippi River
    <class 'str'>
    Quincy
    <class 'str'>
    Palmyra
    a few days
    19th
    Illinois
    <class 'str'>
    Salt River
    <class 'str'>
    John M. Palmer
    13th
    Illinois
    <class 'str'>
    two
    about two weeks
    Thomas Harris
    Florida
    <class 'str'>
    some twenty-five
    Salt River
    <class 'str'>
    some days
    garrison
    nearly a thousand
    week
    twenty-five miles
    two
    night
    the next morning
    an early hour
    Harris
    more than a hundred feet
    Harris
    Illinois
    <class 'str'>
    Harris
    a few days
    Harris
    Florida
    <class 'str'>
    Harris
    Florida
    <class 'str'>
    Salt River
    <class 'str'>
    forty miles
    The next day
    Salt River
    <class 'str'>
    National
    Salt River
    <class 'str'>
    Mexico
    <class 'str'>
    Pope
    State
    Missouri
    <class 'str'>
    Mississippi
    <class 'str'>
    Missouri
    <class 'str'>
    Mexico
    <class 'str'>
    three
    one
    first
    the night
    one
    West Point
    <class 'str'>
    Mexico
    <class 'str'>
    two or
    Mexico
    <class 'str'>
    Springfield
    <class 'str'>
    West Point
    <class 'str'>
    Scott
    Mexican
    the summer of 1846
    Hardee's
    one
    the first
    day
    day to day
    Hardee
    French
    Hardee's
    Scott
    Mexico
    <class 'str'>
    many weeks
    St. Louis
    <class 'str'>
    Illinois
    <class 'str'>
    Congress
    State
    first
    seven
    the next day
    three
    Senate
    a few days
    one
    Lieutenant C. B. Lagow
    St. Louis
    <class 'str'>
    McClellan, Moody
    
    Hillyer
    the day
    Hillyer
    twenties
    one
    Galena
    Presidential
    John A. Rawlins
    State
    Douglas
    Sumter
    Union
    State
    Hillyer
    Lagow
    Vicksburg
    Chattanooga
    the General of the Army
    Rawlins
    Ironton
    Missouri
    <class 'str'>
    State
    the 21st
    Illinois
    <class 'str'>
    Ironton
    about seventy miles
    St. Louis
    <class 'str'>
    B. Gratz
    Missouri
    <class 'str'>
    1872
    ninety days'
    Greenville
    <class 'str'>
    some twenty-five miles
    five thousand
    Confederate
    Colonel
    Brown's
    Brown
    a
    day
    two
    ten days
    Ironton
    Greenville
    <class 'str'>
    Greenville
    <class 'str'>
    ten miles
    Ironton
    two
    the next morning
    Harris
    Missouri
    <class 'str'>
    B. M. Prentiss
    Prentiss
    St. Louis
    <class 'str'>
    the
    same day
    Greenville
    <class 'str'>
    St. Louis
    <class 'str'>
    Jefferson City
    <class 'str'>
    State
    General Sterling Price
    Confederate
    Lexington
    <class 'str'>
    Chillicothe
    Missouri
    <class 'str'>
    Jefferson
    City
    <class 'str'>
    Mulligan
    three years
    Jefferson City
    <class 'str'>
    August
    1861
    six
    months
    a year
    State
    three years
    Union
    National
    two
    Union
    Missouri
    <class 'str'>
    National
    Jefferson City
    <class 'str'>
    a few days
    Lexington
    <class 'str'>
    Booneville
    Chillicothe
    St. Louis
    <class 'str'>
    some twenty miles
    seven or eight days
    Jefferson
    City
    <class 'str'>
    the next day
    Jefferson C. Davis
    Jefferson City
    <class 'str'>
    St. Louis
    <class 'str'>
    about an hour
    Davis
    one
    an hour
    St. Louis
    <class 'str'>
    C. B. Lagow
    the next day
    the next
    day
    Missouri
    <class 'str'>
    St. Louis
    <class 'str'>
    Missouri
    <class 'str'>
    Illinois
    <class 'str'>
    first
    Jeff
    Thompson
    Missouri
    <class 'str'>
    Ironton
    Cape Girardeau
    <class 'str'>
    sixty or seventy miles
    Cape Girardeau
    <class 'str'>
    Jacksonville
    <class 'str'>
    ten miles
    Ironton
    Cairo
    <class 'str'>
    Bird
    Point
    Ohio
    <class 'str'>
    Mississippi
    <class 'str'>
    Mississippi
    <class 'str'>
    Belmont
    <class 'str'>
    eighteen miles
    Cairo
    <class 'str'>
    Cape Girardeau
    <class 'str'>
    Jackson
    Prentiss
    Ironton
    Jackson
    Colonel
    Marsh
    Jackson
    Jackson
    Two or three days
    Cape Girardeau
    <class 'str'>
    Prentiss
    Jackson
    first
    Prentiss
    Jackson
    <class 'str'>
    the night
    Cape Girardeau
    <class 'str'>
    the morning
    Jackson
    Cairo
    <class 'str'>
    Springfield
    <class 'str'>
    the United States
    <class 'str'>
    May 17th, 1861
    Prentiss
    Jackson
    the next morning
    Cape Girardeau
    <class 'str'>
    Jackson
    Jackson
    St. Louis
    <class 'str'>
    Jeff
    Thompson
    Arkansas
    <class 'str'>
    Missouri
    <class 'str'>
    Prentiss
    State
    West
    <class 'str'>
    Missouri
    <class 'str'>
    Mexican
    4th
    Cairo
    <class 'str'>
    Richard Oglesby
    New York
    <class 'str'>
    States
    <class 'str'>
    Missouri
    <class 'str'>
    Kentucky
    <class 'str'>
    Missouri
    <class 'str'>
    Richard J. Oglesby
    Bird
    Point
    The day
    Cairo
    <class 'str'>
    General Fremont
    Columbus
    <class 'str'>
    twenty miles
    Kentucky
    <class 'str'>
    Paducah
    <class 'str'>
    Tennessee
    <class 'str'>
    Cairo
    <class 'str'>
    only a few hours
    Cairo
    <class 'str'>
    Paducah
    <class 'str'>
    about forty-five miles
    6th
    first
    Paducah
    <class 'str'>
    midnight
    early the following morning
    six or eight hours
    Jeff
    Thompson
    Paducah
    <class 'str'>
    National
    that day
    nearly four
    thousand
    Columbus
    <class 'str'>
    ten or fifteen
    Paducah
    <class 'str'>
    two
    one
    Columbus
    <class 'str'>
    noon
    Cairo
    <class 'str'>
    Paducah
    <class 'str'>
    Paducah
    <class 'str'>
    Cape Girardeau
    <class 'str'>
    C. F. Smith
    Cairo
    <class 'str'>
    Tennessee
    <class 'str'>
    Smithland
    Cumberland
    <class 'str'>
    State
    Kentucky
    <class 'str'>
    North
    <class 'str'>
    South
    <class 'str'>
    State
    two
    State
    Columbus
    <class 'str'>
    Hickman
    Mississippi
    <class 'str'>
    National
    Paducah
    <class 'str'>
    Ohio
    <class 'str'>
    Lloyd
    Tilghman
    Confederate
    nearly four thousand
    Confederate
    Kentucky
    <class 'str'>
    National
    Kentucky
    <class 'str'>
    Cairo
    <class 'str'>
    Paducah
    <class 'str'>
    Cairo
    <class 'str'>
    General Fremont
    Camp
    Jackson
    the month of May
    Columbus
    <class 'str'>
    one
    Cape Girardeau
    <class 'str'>
    the next day
    twenty or more miles
    Cairo
    <class 'str'>
    the next day
    one
    the day before
    Barrett
    St. Louis
    <class 'str'>
    Paducah
    <class 'str'>
    November
    the 1st of November
    fewer than 20,000
    two
    Columbus
    <class 'str'>
    Paducah
    <class 'str'>
    November
    October
    General Fremont
    Jefferson City
    <class 'str'>
    General Sterling
    Missouri
    <class 'str'>
    first
    Columbus
    <class 'str'>
    the same quarter
    some 3,000
    about fifty miles
    Cairo
    <class 'str'>
    5th
    Columbus
    <class 'str'>
    Mississippi
    <class 'str'>
    the White River
    <class 'str'>
    Arkansas
    <class 'str'>
    Bird
    Point
    W. H. L. Wallace
    Oglesby
    New Madrid
    Columbus
    <class 'str'>
    Missouri
    <class 'str'>
    C. F. Smith
    Paducah
    <class 'str'>
    Columbus
    <class 'str'>
    a few miles
    Cairo
    <class 'str'>
    Fort Holt
    <class 'str'>
    two
    a little over 3,000
    five
    two
    two
    6th
    about six miles
    Columbus
    <class 'str'>
    Kentucky
    <class 'str'>
    Paducah
    <class 'str'>
    National
    Cairo
    <class 'str'>
    Cairo
    <class 'str'>
    Columbus
    <class 'str'>
    About two o'clock on the morning of the 7th
    Columbus
    <class 'str'>
    the west bank
    <class 'str'>
    Oglesby
    Confederates at Belmont
    Columbus
    <class 'str'>
    Missouri
    <class 'str'>
    Belmont
    <class 'str'>
    Columbus
    <class 'str'>
    an hour
    the west bank
    <class 'str'>
    Mississippi
    <class 'str'>
    Columbus
    <class 'str'>
    Columbus
    <class 'str'>
    Belmont
    <class 'str'>
    Columbus
    <class 'str'>
    Paducah
    <class 'str'>
    Columbus
    <class 'str'>
    the east bank
    <class 'str'>
    the
    east bank
    <class 'str'>
    Columbus
    <class 'str'>
    About eight o'clock
    a mile
    a
    mile
    Belmont
    <class 'str'>
    about four hours
    Belmont
    <class 'str'>
    first
    one
    four
    hours
    two
    Columbus
    Columbus
    <class 'str'>
    first
    Confederates
    Columbus
    <class 'str'>
    first
    first
    fifty yards
    first
    a few hundred yards
    the Mississippi River
    <class 'str'>
    twelve
    The Mississippi River
    <class 'str'>
    the 7th of November, 1861
    three
    two
    first
    Cairo
    <class 'str'>
    Belmont
    <class 'str'>
    Belmont
    <class 'str'>
    485
    About 125
    175
    two
    four
    642
    about 2,500
    about 7,000
    Columbus
    <class 'str'>
    first
    Belmont
    <class 'str'>
    two
    Belmont
    <class 'str'>
    Columbus
    <class 'str'>
    Columbus
    Belmont
    <class 'str'>
    National
    Belmont
    <class 'str'>
    The day
    General Polk's
    Belmont
    <class 'str'>
    West Point
    <class 'str'>
    Mexican
    General Polk's
    Polk
    Yankee
    Belmont
    <class 'str'>
    North
    <class 'str'>
    Oglesby
    three
    thousand
    Cairo
    <class 'str'>
    Columbus
    <class 'str'>
    two or
    one
    Fort Holt
    West Point
    <class 'str'>
    Mexico
    <class 'str'>
    South
    <class 'str'>
    North
    <class 'str'>
    States
    <class 'str'>
    North
    <class 'str'>
    many months
    Army
    Potomac
    second
    four
    Cairo
    <class 'str'>
    the 9th of November
    two days
    Belmont
    <class 'str'>
    H. W. Halleck
    General Fremont
    the Department of the Missouri
    Arkansas
    <class 'str'>
    west Kentucky
    <class 'str'>
    the Cumberland River
    <class 'str'>
    Belmont
    <class 'str'>
    February, 1862
    Columbus
    <class 'str'>
    Mill Springs
    Kentucky
    <class 'str'>
    Tennessee
    <class 'str'>
    Cumberland
    <class 'str'>
    Tennessee
    <class 'str'>
    Tennessee
    <class 'str'>
    Fort
    Heiman
    Fort Henry
    <class 'str'>
    Cumberland
    <class 'str'>
    Fort
    <class 'str'>
    Donelson
    two
    eleven
    miles
    at least two miles
    only seven miles
    Fort Henry
    Muscle Shoals
    Alabama
    <class 'str'>
    Charleston Railroad
    the Tennessee at Eastport
    Mississippi
    <class 'str'>
    Fort Henry
    Fort Donelson
    Nashville
    <class 'str'>
    Kentucky
    <class 'str'>
    two
    Memphis
    <class 'str'>
    Charleston
    <class 'str'>
    Halleck
    the District of South-east
    <class 'str'>
    Missouri
    <class 'str'>
    District
    <class 'str'>
    Cairo
    <class 'str'>
    General C. F. Smith
    Tennessee
    <class 'str'>
    Cumberland
    <class 'str'>
    January, 1862
    McClellan
    Brigadier-General
    Don Carlos
    Buell
    the Department of the Ohio
    Louisville
    <class 'str'>
    S. B. Buckner
    Confederate
    Bowling Green
    Buell
    Columbus
    <class 'str'>
    Fort Henry
    <class 'str'>
    Donelson to Buckner
    Smith
    the west bank
    <class 'str'>
    Tennessee
    <class 'str'>
    Heiman
    Henry
    McClernand
    6,000
    west Kentucky
    <class 'str'>
    Columbus
    <class 'str'>
    the Tennessee River
    <class 'str'>
    McClernand
    more than a week
    Bowling
    Green
    George H. Thomas
    Mill Springs
    Smith
    Fort Heiman
    <class 'str'>
    Fort Henry
    Fort Henry
    Smith
    Tennessee
    <class 'str'>
    Cumberland
    <class 'str'>
    the State of Kentucky
    the 6th of
    January
    St. Louis
    <class 'str'>
    Smith
    St. Louis
    <class 'str'>
    Halleck
    West Point
    <class 'str'>
    Mexican
    Cairo
    <class 'str'>
    Cairo
    <class 'str'>
    Halleck
    Tennessee
    <class 'str'>
    the 28th of January
    Fort Henry
    Tennessee
    <class 'str'>
    Flag
    Foote
    the 29th
    the 1st of February
    Fort Henry
    2d
    February, 1862
    Cairo
    <class 'str'>
    the Mississippi River
    <class 'str'>
    one
    17,000
    Tennessee
    <class 'str'>
    more than half
    McClernand
    one
    McClernand
    nine miles
    Fort Henry
    Seven
    Flag
    Foote
    Paducah
    <class 'str'>
    General C. F. Smith
    Tennessee
    <class 'str'>
    Tennessee
    <class 'str'>
    Cumberland
    <class 'str'>
    February
    Fort Henry
    Essex
    Captain Wm
    Porter
    One
    Porter
    Paducah
    <class 'str'>
    5th
    ten o'clock at night
    5th
    11 A.M.
    6th
    Fort Henry
    <class 'str'>
    two miles
    Donelson
    Dover
    <class 'str'>
    about 2,800
    Donelson
    seventeen
    Fort Henry
    two feet
    several hundred yards
    Fort Heiman
    <class 'str'>
    Fort Henry
    Fort Henry
    Donelson
    eleven miles
    two
    every quarter
    Smith
    west bank
    <class 'str'>
    the night
    5th
    Heiman
    <class 'str'>
    the hour
    Fort Heiman
    close
    quarters
    Fort Henry
    first
    Tilghman
    about one hundred
    Dover
    <class 'str'>
    Donelson
    6th
    Donelson
    Tilghman
    ninety
    Donelson
    two
    Essex
    forty-eight
    nineteen
    navy
    Fort Henry
    Phelps
    the Tennessee River
    <class 'str'>
    Memphis
    <class 'str'>
    Ohio Railroad
    Fort Henry
    Fort Donelson
    <class 'str'>
    the 7th
    the day
    Fort Henry
    <class 'str'>
    one
    about a mile
    Donelson
    Pillow
    Mexico
    <class 'str'>
    Floyd
    Fort Donelson
    <class 'str'>
    two
    one
    Dover
    <class 'str'>
    Donelson
    Fort Donelson
    two miles
    Dover
    <class 'str'>
    1861
    about one hundred acres
    Cumberland
    <class 'str'>
    Hickman
    Cumberland
    <class 'str'>
    Cumberland
    <class 'str'>
    some two
    miles
    one
    about half
    Hickman
    one
    Halleck
    Cairo
    <class 'str'>
    Hunter
    Kansas
    <class 'str'>
    Nelson
    Buell
    the War Department
    the Western
    States
    Halleck
    Fort Donelson
    <class 'str'>
    Buell
    7th
    Fort Donelson
    the next day
    the 10th
    Fort Henry
    Fort Donelson
    <class 'str'>
    Fort Donelson
    15,000
    the 8th
    a month later
    Flag
    Foote
    Cairo
    <class 'str'>
    the Cumberland River
    <class 'str'>
    Eastport
    Florence
    the 12th
    McClernand
    a few miles
    first
    six
    Thayer
    Nebraska
    <class 'str'>
    Donelson
    Tennessee
    <class 'str'>
    Ohio
    <class 'str'>
    Cumberland
    <class 'str'>
    Thayer
    Fort Henry
    15,000
    eight
    noon
    That afternoon
    the next day
    Henry and Heiman
    Lew
    Wallace
    2,500
    Hickman
    McClernand
    Dover
    <class 'str'>
    Cumberland
    <class 'str'>
    Fort Henry
    the 12th and 13th
    Wallace
    Thayer
    the 14th
    National
    15,000
    21,000
    Only one
    each day
    the 13th
    McClernand
    three
    William Morrison
    Fort Donelson
    one
    two
    Captain Walke
    Fort Henry
    the 10th
    Tennessee
    <class 'str'>
    Cumberland
    <class 'str'>
    Donelson
    Carondelet
    Alps
    <class 'str'>
    Walke
    a few miles
    Donelson
    the 12th
    a little after noon
    the 13th
    the day
    the night of the 13th
    Flag
    Foote
    St. Louis
    <class 'str'>
    Louisville
    <class 'str'>
    Pittsburg
    <class 'str'>
    Tyler
    Conestoga
    <class 'str'>
    Thayer
    the
    morning of the 14th
    Thayer
    Wallace
    Fort Henry
    <class 'str'>
    C. F. Smith
    Lew
    Wallace
    Thayer
    the same day
    two
    close
    quarters
    Dover
    <class 'str'>
    Dover
    <class 'str'>
    three in the afternoon of the
    14th
    Foote
    two hundred yards
    one
    the day
    Flag
    Foote
    sixty
    Two
    Richmond
    <class 'str'>
    the night of the 14th of February
    1862
    Fort Donelson
    Two
    this night
    the morning of the 15th
    Flag
    the day before
    days
    weeks
    four to seven miles
    evening
    Mound City
    <class 'str'>
    ten days
    National
    Flag
    the first two days
    the 12th to the 14th
    15,000
    six
    L. Wallace
    2,500
    Fort Henry
    C. F. Smith
    Captain Hillyer
    National
    McClernand
    National
    some four or
    five miles
    about three miles
    Smith
    Wallace
    Smith
    Wallace
    Thayer
    McClernand
    McClernand
    Thayer
    J. D. Webster
    first
    Webster
    Smith's
    15th
    Smith
    Confederates
    the next
    day
    Dover
    <class 'str'>
    the night of the 15th
    Floyd
    War
    Constitution
    the United States
    <class 'str'>
    War
    About a year
    Cabinet
    Buchanan
    about the 1st of
    January, 1861
    United States
    <class 'str'>
    Mexican
    Nashville
    <class 'str'>
    Southern
    all day
    Johnston
    Richmond
    <class 'str'>
    Floyd
    General Buckner
    third
    A. S. Johnston
    Nashville
    <class 'str'>
    Buckner
    Donelson
    Johnston
    Nashville
    <class 'str'>
    Buckner
    Floyd
    Buckner
    Floyd
    Pillow
    Dover
    <class 'str'>
    before morning
    Nashville
    <class 'str'>
    Floyd
    all about 3,000
    Cumberland
    <class 'str'>
    Forrest
    about a thousand
    ford
    Dover
    <class 'str'>
    Smith
    General Buckner
    FORT DONELSON
    February 16, 1862
    S. B. BUCKNER
    Brig
    C. S. A.
    Brigadier-General U. S. Grant
    U. S. Forces
    Fort Donelson
    <class 'str'>
    Camp
    Donelson
    February 16, 1862
    S. B. BUCKNER
    Confederate Army
    Commissioners
    U. S. GRANT
    Brig
    DOVER
    <class 'str'>
    TENNESSEE
    <class 'str'>
    February 16, 1862
    Gen'I U. S. GRANT
    Confederate
    yesterday
    ob't
    S. B. BUCKNER
    Brig
    C. S. A.
    General Buckner
    first
    Buckner
    Dover
    <class 'str'>
    Wallace
    an hour
    General Buckner
    West Point
    <class 'str'>
    Buckner
    Donelson
    5,000
    General
    Buckner
    Nashville
    <class 'str'>
    Fort Henry
    Floyd
    Pillow
    the night
    Forrest
    the preceding night
    fewer than 12,000
    more than
    15,000
    the 15th
    Confederates
    <class 'str'>
    Fort Donelson
    Southern
    Preston Johnston
    17,000
    14,623
    Fort Donelson
    <class 'str'>
    Cairo
    <class 'str'>
    2,000
    McClernand
    Buckner
    Pillow
    Floyd
    the
    night of
    15th
    less than 3,000
    Forrest
    about 1,000
    all night
    Confederate
    Donelson
    the 15th of February, 1862
    21,000
    the day
    Fort Donelson
    27,000
    Confederate
    four or five miles
    the 16th
    Sherman
    Smithland
    <class 'str'>
    the Cumberland River
    <class 'str'>
    Sherman
    Fort Donelson
    <class 'str'>
    North
    <class 'str'>
    South
    <class 'str'>
    Richmond
    <class 'str'>
    Major-General of Volunteers
    Senate
    three
    St. Louis
    <class 'str'>
    Hunter
    Kansas
    <class 'str'>
    Fort Donelson
    Washington
    <class 'str'>
    C. F. Smith
    St. Louis
    <class 'str'>
    Flag
    Tennessee
    <class 'str'>
    Cumberland
    <class 'str'>
    Halleck
    Cairo
    <class 'str'>
    General
    Smith's
    the fall
    National
    South
    <class 'str'>
    one
    Alleghanies
    Chattanooga
    Corinth
    Memphis
    <class 'str'>
    Vicksburg
    <class 'str'>
    North
    <class 'str'>
    tens of thousands
    States
    <class 'str'>
    Confederate
    February, 1862
    Time
    Ohio River
    <class 'str'>
    Fort Donelson
    <class 'str'>
    Clarksville
    <class 'str'>
    Nashville
    <class 'str'>
    Clarksville
    the 21st
    Nashville
    <class 'str'>
    the 1st of
    March
    the Cumberland River
    <class 'str'>
    Fort
    <class 'str'>
    Donelson
    C. F. Smith
    Clarksville
    Henry
    Donelson
    Columbus
    <class 'str'>
    Bowling Green
    Buell
    Nashville
    <class 'str'>
    Clarksville
    Buell
    the
    24th of February
    Nelson
    two
    one
    Cairo
    <class 'str'>
    Buell
    Nashville
    <class 'str'>
    Nashville
    <class 'str'>
    South
    <class 'str'>
    Buell
    Nelson
    Nashville
    <class 'str'>
    Fort Donelson
    The Cumberland River
    <class 'str'>
    Nashville
    <class 'str'>
    Nashville
    <class 'str'>
    the west bank
    <class 'str'>
    Cumberland
    <class 'str'>
    Buell
    Nelson
    Buell
    Nelson
    Buell
    more than two days
    Nashville
    <class 'str'>
    Buell
    Edgefield
    <class 'str'>
    Nashville
    <class 'str'>
    Mitchell
    the
    same day
    Nelson
    Nelson
    Buell
    Nashville
    <class 'str'>
    the 28th
    Clarksville
    Nelson
    General C. F. Smith
    Buell
    NASHVILLE
    February 25, 1862
    GENERAL C. F. SMITH
    Commanding U. S. Forces
    Clarksville
    <class 'str'>
    only 15,000
    four
    Diana
    <class 'str'>
    Woodford
    <class 'str'>
    John Rain
    Autocrat
    <class 'str'>
    five or six days
    ob't
    D. C. BUELL
    Brigadier-General Comd'g
    12 o'clock
    Smith
    Nashville
    <class 'str'>
    Nelson
    Buell
    the day
    Nashville
    <class 'str'>
    early morning
    Nelson
    Clarksville
    <class 'str'>
    Smith
    Buell
    Buell
    twelve miles
    Nashville
    <class 'str'>
    Buell
    Nashville
    <class 'str'>
    Clarksville
    Smith
    Smith
    the same day
    Nashville
    <class 'str'>
    Albert Sidney Johnston
    Confederate
    the Alleghany Mountains
    <class 'str'>
    National
    first
    three
    four
    Johnston
    supreme command
    one
    Washington
    <class 'str'>
    the beginning of 1862
    Johnston
    Mississippi
    <class 'str'>
    Columbus
    <class 'str'>
    Mill Springs
    <class 'str'>
    Columbus
    <class 'str'>
    the Tennessee River
    <class 'str'>
    the west bank
    <class 'str'>
    Cumberland
    <class 'str'>
    Bowling Green
    Mill Springs
    Ohio
    <class 'str'>
    three
    Louisville
    <class 'str'>
    Bowling Green
    Johnston
    National
    Confederate
    West
    <class 'str'>
    George H.
    Thomas
    Mill Springs
    <class 'str'>
    some 300
    Henry
    Heiman
    National
    about 100
    Confederate
    Bowling Green
    Nashville
    <class 'str'>
    the 14th of February
    Donelson
    Buell
    the Army of the Ohio
    Cumberland
    <class 'str'>
    Nashville
    <class 'str'>
    the 24th of the month
    only one
    Nashville
    <class 'str'>
    ten days
    Bowling Green
    Johnston
    Nashville
    <class 'str'>
    Fort Donelson
    <class 'str'>
    the States of Kentucky
    <class 'str'>
    Tennessee
    <class 'str'>
    two
    Fort Donelson
    Confederate
    the night of the 16th
    Johnston
    Floyd
    one
    Pillow
    second
    Nashville
    <class 'str'>
    Donelson
    Johnston
    first
    Richmond
    <class 'str'>
    the 8th of February
    Fort Donelson
    Nashville
    <class 'str'>
    Chattanooga
    Mississippi
    <class 'str'>
    six weeks later
    Cairo
    <class 'str'>
    Halleck
    10th
    February
    Fort Henry
    Donelson
    Donelson
    Cairo
    <class 'str'>
    St. Louis
    <class 'str'>
    Cairo
    <class 'str'>
    Cairo
    <class 'str'>
    Paducah
    <class 'str'>
    Smithland
    Tennessee
    <class 'str'>
    Cumberland
    <class 'str'>
    Cairo
    <class 'str'>
    McClellan
    February 16th
    the day of the surrender
    3d
    March
    2d
    March
    March 1st
    Fort Henry
    <class 'str'>
    Donelson
    Fort Henry
    Eastport
    <class 'str'>
    Mississippi
    <class 'str'>
    Paris
    <class 'str'>
    Tennessee
    <class 'str'>
    Donelson
    4th
    Tennessee
    <class 'str'>
    March 4th
    Halleck
    U. S. GRANT
    Fort Henry
    C. F. Smith
    Fort Henry
    H. W. HALLECK
    first
    Halleck
    6th
    Nashville
    <class 'str'>
    Washington
    <class 'str'>
    first
    Nashville
    <class 'str'>
    Nashville
    <class 'str'>
    the Cumberland River
    <class 'str'>
    Halleck
    Halleck
    McClellan
    Halleck
    Halleck
    <class 'str'>
    Washington
    <class 'str'>
    Nashville
    <class 'str'>
    Bull Run
    McClellan
    less than two weeks
    Donelson
    two
    less than three weeks
    the 13th of March
    the 17th
    Halleck
    <class 'str'>
    the War Department
    Washington
    <class 'str'>
    Washington
    <class 'str'>
    Badeau
    Halleck
    C. F. Smith
    Smith
    Smith
    Halleck
    Washington
    <class 'str'>
    Savannah
    <class 'str'>
    Tennessee
    <class 'str'>
    Smith
    the 17th of March
    about half
    Tennessee
    <class 'str'>
    Savannah
    <class 'str'>
    one
    Crump
    about four miles
    Pittsburg
    <class 'str'>
    five miles
    Crump's
    Corinth
    two
    one
    Memphis
    <class 'str'>
    the Mississippi
    River
    <class 'str'>
    East
    <class 'str'>
    Corinth
    Jackson
    west Tennessee
    <class 'str'>
    Corinth
    Vicksburg
    <class 'str'>
    West
    <class 'str'>
    Tennessee
    <class 'str'>
    Mississippi
    <class 'str'>
    Nashville
    <class 'str'>
    Vicksburg
    <class 'str'>
    Savannah
    <class 'str'>
    Pittsburg
    <class 'str'>
    Corinth
    Johnston
    Buell
    the Army of the Ohio
    Pittsburg
    <class 'str'>
    about twenty miles
    Corinth
    Hamburg
    <class 'str'>
    four miles
    a mile
    two
    Hamburg
    <class 'str'>
    Army
    Ohio
    <class 'str'>
    Pittsburg
    <class 'str'>
    Hamburg
    <class 'str'>
    Corinth
    some eight miles
    Savannah
    <class 'str'>
    Sherman
    Army
    Tennessee
    <class 'str'>
    Eastport
    <class 'str'>
    thirty miles
    Corinth
    the 17th of March
    Tennessee River
    <class 'str'>
    five
    Generals C. F. Smith
    McClernand
    L. Wallace
    Hurlbut
    Sherman
    W. H. L.
    
    Wallace
    Smith
    General
    Smith
    Reinforcements
    daily
    first
    Prentiss
    Buell
    Nashville
    <class 'str'>
    40,000
    the
    19th of March
    Columbia
    <class 'str'>
    Tennessee
    <class 'str'>
    eighty-five miles
    Pittsburg
    <class 'str'>
    Corinth
    McPherson
    Tennessee
    <class 'str'>
    Johnston
    the 1st of April
    2d
    Johnston
    Corinth
    4th
    six or
    seven
    some five miles
    Pittsburg
    <class 'str'>
    Corinth
    Buckland
    Sherman
    Buckland
    some three
    nightfall Sherman
    Mobile
    <class 'str'>
    Ohio
    <class 'str'>
    Crump
    Pittsburg
    <class 'str'>
    Crump's
    Wallace
    Wallace
    the day
    Pittsburg
    <class 'str'>
    Savannah
    <class 'str'>
    Pittsburg
    <class 'str'>
    Buell
    daily
    Savannah
    <class 'str'>
    a few
    days
    3d
    April
    Pittsburg
    <class 'str'>
    an hour
    the morning
    Friday
    4th
    the day
    Buckland
    <class 'str'>
    The night
    W. H. L. Wallace
    McPherson
    the few preceding days
    two or three days
    5th
    Nelson
    Buell
    Savannah
    <class 'str'>
    the east bank
    <class 'str'>
    Crump
    Pittsburg
    <class 'str'>
    Buell
    Savannah
    <class 'str'>
    the next day
    Pittsburg
    <class 'str'>
    several days
    the day
    Buell
    5th
    Pittsburg
    <class 'str'>
    Buell
    Savannah
    <class 'str'>
    Crump
    Lew
    Wallace
    Crump
    eight
    Pittsburg
    <class 'str'>
    Crump's
    Captain Baxter
    Wallace
    Pittsburg
    <class 'str'>
    Captain Baxter
    one
    P.M.
    <class 'str'>
    Wallace
    two
    McPherson
    Rowley
    Purdy
    <class 'str'>
    Bethel
    Pittsburg
    <class 'str'>
    several miles
    first
    Pittsburg
    <class 'str'>
    two
    Snake Creek
    <class 'str'>
    Wallace
    two
    Wallace
    the first day's
    Wallace
    Captain Baxter
    Pittsburg
    <class 'str'>
    Purdy
    <class 'str'>
    Owl Creek
    Sherman
    Pittsburg
    <class 'str'>
    one
    three
    Wallace
    the 6th of April, 1862
    Some two or
    three miles
    Pittsburg
    <class 'str'>
    Shiloh
    Snake
    <class 'str'>
    Lick creeks
    <class 'str'>
    Tennessee
    <class 'str'>
    Pittsburg
    <class 'str'>
    Sherman
    McClernand
    Sherman
    Henry
    Donelson
    McClernand
    Stuart
    one
    Sherman
    Prentiss
    <class 'str'>
    C. F. Smith
    Smith
    Savannah
    <class 'str'>
    Brigadier-General
    W. H. L. Wallace
    Mexican
    Henry
    Donelson
    Wallace
    the first day's
    Lick
    Creek
    <class 'str'>
    Owl Creek
    Snake Creek
    <class 'str'>
    National
    Confederate
    Sherman
    National
    Pittsburg
    <class 'str'>
    National
    a mile
    the morning
    one
    6th
    Prentiss
    about
    2,200
    Badeau
    four o'clock
    6th
    Prentiss
    the hour
    that day
    about half-past
    four or later
    thousands
    Prentiss
    all-day
    thousands
    Confederate
    a few minutes
    Prentiss
    all day
    Snake Creek
    <class 'str'>
    Lick Creek
    Tennessee
    <class 'str'>
    Pittsburg
    <class 'str'>
    no hour
    the day
    Southern
    Northern
    Three
    five
    Sunday
    States
    <class 'str'>
    two
    first
    two
    first
    first
    Shiloh
    Sunday
    one
    Sherman
    first
    McClernand
    Sherman
    two
    McClernand
    that day
    6th
    Sherman
    Shiloh
    6th
    third
    the day
    one
    Buell
    the
    hour
    as many as four or five
    thousand
    Buell
    Savannah
    <class 'str'>
    Buell
    Buell
    Confederate
    Tennessee
    <class 'str'>
    Mississippi
    <class 'str'>
    Confederate
    Johnston
    as high as 20,000
    Sunday
    Pittsburg
    <class 'str'>
    J. D. Webster
    twenty
    Tennessee
    <class 'str'>
    McClernand
    Snake Creek
    <class 'str'>
    two
    three
    the day
    W. H.
    L. Wallace
    Shiloh
    Snake Creek
    <class 'str'>
    Crump
    Pittsburg
    <class 'str'>
    Sherman
    Wallace
    Sherman
    night
    Lew
    <class 'str'>
    Wallace
    The Tennessee River
    <class 'str'>
    Tyler
    Lexington
    <class 'str'>
    Gwin
    Shirk
    Webster
    Buell
    the west bank
    <class 'str'>
    Tennessee
    <class 'str'>
    Buell
    some minutes
    Lew
    Wallace
    5,000
    the day
    night
    Wallace
    Nelson
    Shiloh
    that first day
    Buell
    the 6th of April
    two
    one
    36th
    Indiana
    <class 'str'>
    Army
    Tennessee
    <class 'str'>
    that day
    at least 7,000
    two
    three
    Buell
    the west bank
    <class 'str'>
    Pittsburg
    <class 'str'>
    6th
    next day
    Sherman
    Fort Donelson
    Shiloh
    Wallace
    Buell
    the night of the
    6th
    Nelson
    Buell
    the
    morning
    Two
    Crittenden
    McCook
    Savannah
    <class 'str'>
    the west bank
    <class 'str'>
    Buell
    a few hundred yards
    Friday
    night
    midnight
    the morning of the
    7th
    Confederates
    Buell
    fifteen minutes
    the morning of the 7th
    Lew
    Wallace
    Sherman
    McClernand
    Hurlbut
    Nelson
    Buell
    Crittenden
    Nelson
    McCook
    Buell
    Buell
    the entire day
    This day
    Union
    all day
    the day before
    Corinth
    Sherman
    McClernand
    About three o'clock
    CHARGE
    W. H. L. Wallace
    the first day's
    Shiloh
    General
    Lew
    Wallace
    the morning
    5th
    Confederates
    the Mobile & Ohio
    Crump
    Pittsburg
    <class 'str'>
    Shiloh
    <class 'str'>
    Lew
    Wallace
    Shiloh
    Crump
    Adamsville
    <class 'str'>
    Pittsburg
    <class 'str'>
    Purdy
    two
    nearly a mile
    Owl Creek
    General Lew
    Wallace
    W. H. L. Wallace
    April 5th
    the
    same day
    4th
    W. H. L.
    Wallace's
    two
    Lew
    Wallace
    Shiloh
    first
    Wallace
    Pittsburg
    <class 'str'>
    Wallace
    Wallace
    First
    Crump
    Second
    two miles
    Third
    Wallace
    First
    Third
    Second
    Wallace
    U. S. GRANT.
    MOUNT MACGREGOR
    NEW YORK
    <class 'str'>
    June 21, 1885
    this second day
    afternoon
    McPherson
    Hawkins
    about a minute
    Hawkins
    McPherson
    a few minutes
    three
    one
    one
    some days previous
    two days
    Buell
    a few weeks
    Buell
    Buell
    Shiloh
    the Century Magazine
    McCook
    Buell
    Monday, April 7th
    Badeau
    McCook
    twenty-two miles
    Savannah
    <class 'str'>
    the morning of
    6th
    a few days
    the day before
    the second day
    the Army of the
    
    Tennessee
    <class 'str'>
    Sherman
    McCook
    General McCook
    Century
    one
    several miles
    the day
    About five miles
    West
    <class 'str'>
    East
    <class 'str'>
    the second day
    Confederates
    the day
    National
    Confederate
    one
    several years
    eight or ten feet
    more than half
    the first day
    two or three
    Yankee
    Confederate
    Sherman
    McClernand
    two
    Union
    Shiloh
    daily
    Buell
    two years
    West Point
    <class 'str'>
    Mexican
    several
    years
    years
    disciplinarian
    One
    Buell
    1864
    War
    Buell
    the summer of 1865
    North
    <class 'str'>
    Buell
    one
    Buell
    the New York World
    Albert Sidney Johnston
    Confederate
    the first day
    National
    Johnston
    Mexican
    West Point
    <class 'str'>
    Johnston
    <class 'str'>
    Kentucky
    <class 'str'>
    Tennessee
    <class 'str'>
    Richmond
    <class 'str'>
    Jefferson Davis
    Johnston
    Johnston
    Corinth
    Shiloh
    <class 'str'>
    Tennessee
    <class 'str'>
    Buell
    the Ohio River
    <class 'str'>
    Johnston
    Corinth
    2d
    April
    6th
    march
    less than twenty miles
    second
    two
    first
    National
    the
    Confederates
    <class 'str'>
    second
    Johnston
    Beauregard
    the morning
    5th
    evening
    the same day
    the morning
    6th
    National
    Shiloh
    Johnston
    Beauregard
    Johnston
    <class 'str'>
    Corinth
    Confederate
    Shiloh
    Johnston
    IFS
    Johnston
    no hour
    the day
    an earlier hour
    Shiloh
    Preston Johnston
    the Tennessee River
    <class 'str'>
    two miles
    Confederate
    twelve hours
    National
    National
    eight o'clock
    Confederate
    the 6th of April
    the first day
    National
    many days
    the second day
    Confederate
    Shiloh
    the Armies of the Tennessee
    Ohio
    <class 'str'>
    Army
    Tennessee
    <class 'str'>
    6th
    night
    night
    three
    Nelson
    Confederates
    Shiloh
    Confederate
    Union
    Shiloh
    American
    Southern
    Northern
    <class 'str'>
    the first day
    first
    one
    Confederates
    second
    the second day
    first
    Prentiss
    <class 'str'>
    Monday
    Sunday
    6th
    Sherman
    seven
    McClernand
    six
    eight
    Hurlbut
    two
    seven
    McClernand three
    Army
    Ohio
    <class 'str'>
    Shiloh
    the
    morning
    6th
    33,000
    Wallace
    5,000
    nightfall
    40,955
    South
    <class 'str'>
    6th
    more than 25,000
    7th
    Buell
    20,000
    two
    Thomas
    Wood's
    the two days'
    1,754
    8,408
    2,885
    2,103
    the Army of the Ohio
    Beauregard
    10,699
    1,728
    8,012
    957
    McClernand
    Sherman
    4,000
    Beauregard
    Confederate
    6th
    40,000
    the two days
    10,699
    only 20,000
    the morning of the 7th
    navy
    Shiloh
    the first
    day
    two
    nightfall
    every fifteen minutes
    Confederate
    Shiloh I
    thousands
    Donelson
    Henry
    more than 21,000
    Columbus
    <class 'str'>
    Hickman
    <class 'str'>
    Kentucky
    <class 'str'>
    Clarksville
    <class 'str'>
    Nashville
    <class 'str'>
    Tennessee
    <class 'str'>
    two
    Tennessee
    <class 'str'>
    Cumberland
    <class 'str'>
    Confederate
    Memphis
    <class 'str'>
    Chattanooga
    Knoxville
    <class 'str'>
    Atlantic
    <class 'str'>
    Union or Secession
    Confederate
    Northern
    <class 'str'>
    Shiloh
    Pittsburg
    <class 'str'>
    Confederate
    Sherman
    Badeau
    Prentiss
    Halleck
    A few days
    Halleck
    Pittsburg
    <class 'str'>
    Shiloh I
    one
    Buell
    the War Department
    Halleck
    Pittsburg
    <class 'str'>
    the 11th of
    April
    the 21st
    Pope
    30,000
    Island Number Ten
    the Mississippi River
    <class 'str'>
    Hamburg
    <class 'str'>
    five miles
    Pittsburg
    <class 'str'>
    Halleck
    <class 'str'>
    three
    the Army of the Ohio
    Buell
    Army
    Mississippi
    <class 'str'>
    Pope
    the Army of the Tennessee
    George H. Thomas
    Buell
    Army
    Tennessee
    <class 'str'>
    McClernand
    Lew
    Wallace
    McClernand
    Lew
    <class 'str'>
    Wallace
    Buell
    Army
    Ohio
    <class 'str'>
    Pope
    Army
    Mississippi
    <class 'str'>
    second
    Shiloh
    the Army of the Tennessee
    the Army of the Ohio
    Buell
    Halleck
    Washington
    <class 'str'>
    Corinth
    Owl Creek
    Corinth
    the 30th of April
    Mobile
    <class 'str'>
    Ohio
    <class 'str'>
    Corinth
    Monterey
    twelve miles
    Pittsburg
    <class 'str'>
    Corinth
    Mississippi
    <class 'str'>
    Pittsburg
    <class 'str'>
    about nineteen miles
    twenty-two
    four miles
    the States of Tennessee
    <class 'str'>
    Mississippi
    <class 'str'>
    Mississippi
    <class 'str'>
    Chattanooga
    Mobile
    <class 'str'>
    Ohio
    <class 'str'>
    Columbus
    <class 'str'>
    Mobile
    <class 'str'>
    Pittsburg
    <class 'str'>
    Corinth
    1862
    two
    some four miles
    the Tuscumbia River
    <class 'str'>
    Corinth
    the west bank
    <class 'str'>
    Corinth
    Donelson
    Nashville
    <class 'str'>
    Pittsburg
    <class 'str'>
    Shiloh
    Pope
    Shiloh
    Corinth
    Confederates
    Henry
    Donelson
    Bowling Green
    Columbus
    <class 'str'>
    Nashville
    <class 'str'>
    Shiloh
    Kentucky
    <class 'str'>
    Tennessee
    <class 'str'>
    South
    <class 'str'>
    A. S. Johnston
    the same
    quarter
    Shiloh
    Van Dorn
    Shiloh
    17,000
    Interior
    Corinth
    Beauregard
    the
    month of
    May, 1862
    50,000
    70,000
    120,000
    Corinth
    50,000
    the 30th of April
    Shiloh
    Roads
    the Tennessee River
    <class 'str'>
    Corinth
    one
    Halleck
    Pope
    3d
    May
    Seven Mile Creek
    Farmington
    <class 'str'>
    four miles
    Corinth
    Farmington
    <class 'str'>
    that day
    Pope
    the 8th of May
    Farmington
    <class 'str'>
    two
    4th
    May
    Monterey
    twelve miles
    the 25th
    May
    two
    thirty feet
    five miles
    Corinth
    four
    two
    One
    as much as two miles
    National
    a mile
    Sherman
    two
    the 28th of
    May
    that day
    Corinth
    Thomas
    Mobile
    <class 'str'>
    Ohio
    <class 'str'>
    Pope
    Memphis
    <class 'str'>
    Charleston
    <class 'str'>
    Corinth
    Some days
    Army
    Mississippi
    <class 'str'>
    night
    Pope
    the 28th of May
    Logan
    Mobile
    <class 'str'>
    Ohio
    <class 'str'>
    several days
    Corinth
    Corinth
    several days
    Beauregard
    Corinth
    26th
    May
    the 29th
    the 30th of May
    Halleck
    that morning
    Corinth
    National
    Confederate
    Yankees
    Confederates
    Quaker
    Corinth
    National
    MORALE
    Confederate
    Corinth
    the Army of the Tennessee
    Corinth
    Corinth
    two days'
    Shiloh
    Halleck
    Corinth
    one
    two
    three miles
    100,000
    Corinth
    National
    Pope
    Buell
    Buell
    two
    some thirty miles
    the 10th of June
    Corinth
    Army
    Tennessee
    <class 'str'>
    Confederates
    West Tennessee
    <class 'str'>
    6th of June
    National
    Memphis
    <class 'str'>
    Mississippi
    <class 'str'>
    Columbus
    <class 'str'>
    Corinth
    Donelson
    Clarksville
    <class 'str'>
    Nashville
    <class 'str'>
    the Cumberland
    River
    the Tennessee River
    <class 'str'>
    Eastport
    <class 'str'>
    New
    Orleans
    <class 'str'>
    Baton Rouge
    <class 'str'>
    Richmond
    <class 'str'>
    Vicksburg
    <class 'str'>
    first
    Mississippi
    <class 'str'>
    Memphis
    <class 'str'>
    Baton Rouge
    <class 'str'>
    Corinth
    80,000
    Buell
    the Army of the Ohio
    Memphis
    <class 'str'>
    Charleston
    <class 'str'>
    Chattanooga
    two
    three
    Nashville
    <class 'str'>
    Chattanooga
    <class 'str'>
    Bragg
    Tennessee
    <class 'str'>
    Kentucky
    <class 'str'>
    Stone River
    <class 'str'>
    Chickamauga
    Knoxville
    <class 'str'>
    Chattanooga
    Corinth
    Atlanta
    <class 'str'>
    Vicksburg
    <class 'str'>
    Corinth
    Mississippi
    <class 'str'>
    Corinth
    Halleck
    <class 'str'>
    Memphis
    <class 'str'>
    Donelson
    Corinth
    Halleck
    Sherman
    Memphis
    <class 'str'>
    the 21st of
    June
    one
    two or
    three
    some twenty-five
    La Grange
    <class 'str'>
    La Grange
    <class 'str'>
    Memphis
    <class 'str'>
    forty-seven miles
    two
    La Grange
    General Hurlbut
    General Hurlbut
    a very pleasant afternoon
    Southern
    Mississippi
    <class 'str'>
    that year
    three hundred
    Yankee
    23d of June, 1862
    La Grange
    <class 'str'>
    Memphis
    <class 'str'>
    an early hour
    noon
    twenty miles
    Memphis
    <class 'str'>
    About a mile
    La Grange
    <class 'str'>
    Memphis
    <class 'str'>
    several hundred feet
    Memphis
    <class 'str'>
    twenty miles
    Memphis
    <class 'str'>
    De Loche
    Smith
    De Loche
    Jackson
    De Loche
    Jackson
    De Loche
    Smith
    Memphis
    <class 'str'>
    the day
    Memphis
    <class 'str'>
    Jackson
    day
    two
    one
    Jackson
    six
    seven
    miles
    Memphis
    <class 'str'>
    Charleston
    <class 'str'>
    the house of Mr.
    De Loche
    La Grange
    Memphis
    <class 'str'>
    three-quarters
    three-quarters
    a mile
    Jackson
    A day
    De Loche
    Memphis
    <class 'str'>
    Jackson
    summer
    Manitou Springs
    <class 'str'>
    Colorado
    <class 'str'>
    Memphis
    <class 'str'>
    South
    <class 'str'>
    Dover
    <class 'str'>
    Fort Donelson
    Pittsburg
    <class 'str'>
    Corinth
    Yankee
    hours
    Two
    First
    Memphis
    <class 'str'>
    National
    one
    Army
    Second
    the Confederate Congress
    South
    <class 'str'>
    Southerners
    Northern
    Memphis
    <class 'str'>
    first
    two
    first
    Christian
    Union
    Northern
    first
    second
    North
    <class 'str'>
    Memphis
    <class 'str'>
    Confederate
    the 11th of July
    Halleck
    Washington
    <class 'str'>
    the same day
    Corinth
    Memphis
    <class 'str'>
    Corinth
    the 15th of the month
    Halleck
    the 17th of
    July
    Corinth
    Halleck
    West
    
    <class 'str'>
    Tennessee
    <class 'str'>
    the 25th of October
    Halleck
    the Department of the Mississippi
    Chattanooga
    West Tennessee
    <class 'str'>
    Kentucky
    <class 'str'>
    Cumberland River
    <class 'str'>
    Buell
    the Army of the Ohio
    Chattanooga
    Memphis
    <class 'str'>
    Charleston
    <class 'str'>
    Halleck
    Mobile
    Ohio
    <class 'str'>
    Columbus
    <class 'str'>
    Jackson
    <class 'str'>
    Tennessee
    <class 'str'>
    Grand Junction
    Memphis
    <class 'str'>
    120,000
    Corinth
    the 30th of May
    One
    first
    Corinth
    the months of May and June
    a few
    days
    Donelson
    Clarksville
    <class 'str'>
    Nashville
    <class 'str'>
    Corinth
    Mobile
    Ohio
    <class 'str'>
    Rienzi
    <class 'str'>
    Corinth
    Columbus
    <class 'str'>
    Mississippi
    <class 'str'>
    Jackson
    <class 'str'>
    Tennessee
    <class 'str'>
    Bolivar
    <class 'str'>
    La Grange
    Memphis
    <class 'str'>
    Army
    Tennessee
    <class 'str'>
    Van
    Dorn
    thirty-five
    Missouri
    <class 'str'>
    Corinth
    Bolivar
    Memphis
    <class 'str'>
    National
    Army
    Tennessee
    <class 'str'>
    the fall of
    Corinth
    Memphis
    <class 'str'>
    Columbus
    <class 'str'>
    Memphis
    <class 'str'>
    Mississippi
    <class 'str'>
    Columbus
    <class 'str'>
    Columbus
    <class 'str'>
    Memphis
    <class 'str'>
    three or four days
    at least two days
    Sherman
    the two months
    Halleck
    Mexican
    July
    Ross
    Bolivar
    <class 'str'>
    Jackson
    <class 'str'>
    Corinth
    the 27th
    the Hatchie River
    <class 'str'>
    eight miles
    Bolivar
    <class 'str'>
    the 30th
    P. H.
    Sheridan
    Bragg
    Rome
    <class 'str'>
    Georgia
    <class 'str'>
    Mobile
    <class 'str'>
    Chattanooga
    Rome
    <class 'str'>
    Holly Springs
    <class 'str'>
    Mississippi
    <class 'str'>
    Grand Junction
    Buell
    Bragg
    Chattanooga
    Buell
    National
    Bragg
    2d
    August
    Washington
    <class 'str'>
    Joliet
    Illinois
    <class 'str'>
    National
    the 14th of August
    two
    Buell
    the same day
    Decatur
    <class 'str'>
    the 22d
    Rodney Mason
    Clarksville
    six
    Mason
    first
    Shiloh
    Clarksville
    Donelson
    Clarksville
    Donelson
    Donelson
    South
    <class 'str'>
    the
    treasury department
    the 30th of August
    M. D. Leggett
    Bolivar
    <class 'str'>
    20th
    29th
    Ohio
    <class 'str'>
    about 4,000
    more than one hundred
    the 1st of September
    Medon
    about
    fifty
    only two
    fifteen
    the same day
    Colonel
    Dennis
    less than 500
    two
    a few
    miles
    Medon
    179
    forty-five
    September
    Buell
    Jackson
    Bolivar
    4th
    Granger
    Louisville
    <class 'str'>
    Kentucky
    <class 'str'>
    Buell
    Corinth
    the 10th of June
    Bragg
    Beauregard
    one
    Tupelo
    <class 'str'>
    the 27th of June
    Buell
    about seventeen days'
    eighteen days
    Chattanooga
    National
    Nashville
    <class 'str'>
    Chattanooga
    North
    <class 'str'>
    Buell
    first
    the Army of the Ohio
    the Army of the Mississippi
    four
    Granger
    4th
    Corinth
    P. H. Sheridan
    Sheridan
    first
    eleven years
    4th
    Pacific
    <class 'str'>
    May, 1861
    East
    <class 'str'>
    Missouri
    <class 'str'>
    Halleck
    Indians
    Pacific
    <class 'str'>
    Missouri
    <class 'str'>
    Sheridan
    Halleck
    April
    1862
    Sheridan
    Corinth
    2d
    Michigan
    <class 'str'>
    Blair
    Michigan
    <class 'str'>
    Halleck
    State
    Sheridan
    Corinth
    the Army of the
    
    Mississippi
    <class 'str'>
    Booneville
    <class 'str'>
    the 1st of July
    two
    three
    Sheridan
    Louisville
    <class 'str'>
    Buell
    the night
    Sheridan
    September 4th
    two
    Army
    Mississippi
    <class 'str'>
    Corinth
    <class 'str'>
    Rienzi
    <class 'str'>
    Jacinto
    <class 'str'>
    Danville
    Corinth
    Davies
    two
    McArthur
    Rosecrans
    Ord
    Bethel to
    Humboldt
    Mobile
    <class 'str'>
    Ohio
    <class 'str'>
    Jackson
    <class 'str'>
    Bolivar
    <class 'str'>
    the Mississippi
    Central
    the Hatchie River
    <class 'str'>
    Sherman
    Memphis
    <class 'str'>
    two
    Brownsville
    <class 'str'>
    the Hatchie River
    <class 'str'>
    Memphis
    Ohio
    <class 'str'>
    Brownsville
    <class 'str'>
    Memphis
    <class 'str'>
    a
    few hours
    Corinth
    Bolivar
    <class 'str'>
    Jackson
    less than twenty-four hours
    Brownsville
    <class 'str'>
    Bolivar
    the 7th of September
    Van Dorn
    Corinth
    One
    Memphis
    <class 'str'>
    Bolivar
    <class 'str'>
    first
    Bragg
    <class 'str'>
    Middle Tennessee
    <class 'str'>
    General
    Pope
    Maryland
    <class 'str'>
    Buell
    Louisville
    <class 'str'>
    Bragg
    Confederate
    Buell
    less than 50,000
    Cairo
    <class 'str'>
    Alleghanies
    <class 'str'>
    East
    <class 'str'>
    Nashville
    <class 'str'>
    first
    West Tennessee
    <class 'str'>
    the end of the second year
    East
    <class 'str'>
    Maryland
    <class 'str'>
    State
    West
    <class 'str'>
    Kentucky
    <class 'str'>
    State
    1862
    Washington
    <class 'str'>
    11th
    September
    Rosecrans
    Corinth
    the
    12th
    Murphy
    Wisconsin
    <class 'str'>
    Corinth
    the 13th of September
    Iuka
    <class 'str'>
    about twenty miles
    Corinth
    Memphis
    <class 'str'>
    Charleston
    <class 'str'>
    Murphy
    Tennessee
    <class 'str'>
    Bragg
    Washington
    <class 'str'>
    East
    <class 'str'>
    Middle Tennessee
    <class 'str'>
    Corinth
    Tennessee
    <class 'str'>
    Bolivar
    Jackson
    Corinth
    Jackson
    twenty-four hours
    four
    hours
    8,000
    Ord
    Rosecrans
    Corinth
    about 9,000
    Van Dorn
    about a four days'
    Corinth
    Van Dorn
    Corinth
    Rosecrans
    Iuka
    <class 'str'>
    Memphis
    <class 'str'>
    Charleston
    <class 'str'>
    General Ord's
    Burnsville
    about seven miles
    Iuka
    <class 'str'>
    Rosecrans
    Corinth
    Jacinto
    <class 'str'>
    Jacinto
    <class 'str'>
    Fulton
    Iuka
    <class 'str'>
    Rosecrans
    Bear Creek
    a few miles
    Fulton
    September, 1862
    Tennessee
    <class 'str'>
    Ord
    Price
    National
    Iuka
    <class 'str'>
    the morning of the 18th of September
    Ord
    Burnsville
    the day
    the next morning
    Rosecrans
    the
    morning of the 19th
    two
    all three quarters
    Jacinto
    <class 'str'>
    Rienzi
    <class 'str'>
    Van Dorn
    Corinth
    Burnsville
    <class 'str'>
    Ord
    Van Dorn
    Corinth
    Iuka
    7,000
    8,000
    Burnsville
    Ord
    two
    Burnsville
    one
    the next morning
    Rosecrans
    midnight
    Jacinto
    <class 'str'>
    twenty-two miles
    Iuka
    <class 'str'>
    Jacinto
    <class 'str'>
    Iuka
    <class 'str'>
    two o'clock
    twenty miles
    Ord
    Rosecrans
    the 19th
    Ord
    Burnsville
    A couple of hours
    19th
    Rosecrans
    Barnets
    Jacinto
    Iuka
    Fulton
    Jacinto
    one
    Ord
    Rosecrans
    Burnsville
    Rosecrans
    Jacinto
    <class 'str'>
    Burnsville
    a late hour of the night
    the afternoon
    Ord
    early in the morning
    The next
    Rosecrans
    Iuka
    <class 'str'>
    Ord
    Rosecrans
    Fulton
    the night
    Iuka
    <class 'str'>
    Rosecrans
    a few miles
    a few miles
    Iuka
    Rosecrans
    the 19th of September
    H. Thomas
    Buell
    Memphis
    <class 'str'>
    Charleston
    <class 'str'>
    Corinth
    Chewalla and Grand
    Junction
    two
    Bolivar
    the Mississippi
    Central
    Bolivar
    twenty
    Bolivar
    Jackson
    Corinth
    Davis
    Mississippi
    <class 'str'>
    the 30th
    Van Dorn
    the Mississippi River
    <class 'str'>
    Memphis
    <class 'str'>
    Helena
    <class 'str'>
    Arkansas
    <class 'str'>
    Mississippi Central
    <class 'str'>
    Van Dorn
    Van Dorn
    Memphis
    <class 'str'>
    one
    the 1st of October
    Corinth
    Van Dorn
    Lovell
    Villepigue
    Corinth
    3d
    Memphis
    <class 'str'>
    Charleston
    <class 'str'>
    Mobile
    <class 'str'>
    Ohio
    <class 'str'>
    Corinth
    the night
    3d
    McPherson
    Jackson
    Rosecrans
    Corinth
    Hurlbut
    Bolivar
    <class 'str'>
    Van Dorn
    Corinth
    Hurlbut
    the evening
    3d
    4th
    Rosecrans
    Corinth
    three or
    four
    Corinth
    National
    Halleck
    Rosecrans
    McPherson
    Hurlbut
    Van Dorn's
    McPherson
    Rosecrans
    Hurlbut
    Rosecrans
    first
    4,000
    Ord
    Hurlbut
    4th
    Van
    Dorn's
    Hatchie
    ten miles
    Corinth
    Hurlbut
    Rosecrans
    the morning
    5th
    march
    Two or three hours
    the next day
    Rosecrans
    Van Dorn
    Ord
    Chewalla
    Hatchie
    Van Dorn's
    Rosecrans
    Jonesboro
    Ripley
    <class 'str'>
    Rosecrans
    Van Dorn
    Corinth
    Corinth
    315
    1,812
    232
    Rosecrans
    1,423
    2,225
    Hackelman
    Oglesby
    Corinth
    North
    <class 'str'>
    Vicksburg
    <class 'str'>
    the 23d of October
    Pemberton
    Holly Springs
    <class 'str'>
    Alabama
    <class 'str'>
    Texas
    <class 'str'>
    Rosecrans
    Buell
    Middle Tennessee
    <class 'str'>
    Rosecrans
    48,500
    4,800
    Kentucky
    <class 'str'>
    Illinois
    <class 'str'>
    7,000
    Memphis
    <class 'str'>
    19,200
    Mound City
    <class 'str'>
    17,500
    Corinth
    McClernand
    Washington
    <class 'str'>
    Mississippi
    <class 'str'>
    the 25th of October
    Department
    Tennessee
    <class 'str'>
    2d
    November
    two and a half months
    26th
    October
    McPherson
    C. S. Hamilton
    Major-Generals
    Colonels C. C. Marsh
    20th
    <class 'str'>
    Illinois
    <class 'str'>
    M. M. Crocker
    13th
    Iowa J. A. Mower
    11th
    Missouri
    <class 'str'>
    M.
    D. Leggett
    Ohio
    <class 'str'>
    J. D. Stevenson
    7th
    Missouri
    <class 'str'>
    John E.
    Smith
    45th Illinois
    <class 'str'>
    first
    Memphis
    <class 'str'>
    the Southern States
    <class 'str'>
    Shreveport
    <class 'str'>
    Louisiana
    <class 'str'>
    Mississippi
    <class 'str'>
    Vicksburg
    <class 'str'>
    Vicksburg
    2d
    November
    Grand Junction
    three
    Corinth
    two
    Bolivar
    <class 'str'>
    Jackson
    <class 'str'>
    Tennessee
    <class 'str'>
    Holly Springs
    <class 'str'>
    Grenada
    <class 'str'>
    Mobile
    <class 'str'>
    Ohio
    <class 'str'>
    about twenty-five
    Corinth
    <class 'str'>
    Columbus
    <class 'str'>
    Kentucky
    <class 'str'>
    the Mississippi Central
    <class 'str'>
    Bolivar
    Mobile
    <class 'str'>
    Ohio
    <class 'str'>
    Memphis
    <class 'str'>
    Charleston
    <class 'str'>
    Bear Creek
    <class 'str'>
    the Mississippi River
    <class 'str'>
    Cairo
    <class 'str'>
    Memphis
    <class 'str'>
    about 30,000
    Pemberton
    McPherson
    General C. S.
    
    Hamilton
    Sherman
    Memphis
    <class 'str'>
    Pemberton
    Tallahatchie
    Holly Springs
    <class 'str'>
    the 8th
    Grand Junction
    La Grange
    <class 'str'>
    seven or eight miles
    Bolivar
    <class 'str'>
    Washington
    <class 'str'>
    first
    Freedman's Bureau
    Grand Junction
    many
    thousands
    ten years
    Chaplain
    Eaton
    many years
    United States
    <class 'str'>
    Education
    twelve
    Mississippi River
    <class 'str'>
    McClernand
    the Mississippi River
    <class 'str'>
    Two
    the 12th
    Halleck
    The next day
    Holly Springs
    <class 'str'>
    Tallahatchie
    Columbus
    <class 'str'>
    Kentucky
    <class 'str'>
    La Grange
    Grand
    Junction
    the 15th of November
    Holly Springs
    <class 'str'>
    Sherman
    Columbus
    <class 'str'>
    forty-seven miles
    Columbus
    Sherman
    two
    the Mississippi
    Central railroad
    the 29th
    Cottage Hill
    <class 'str'>
    ten miles
    Oxford
    three
    only four
    Memphis
    <class 'str'>
    Halleck
    Helena
    <class 'str'>
    Arkansas
    <class 'str'>
    Mississippi
    <class 'str'>
    Pemberton
    Generals Hovey
    C. C.
    Washburn
    Tallahatchie
    Pemberton
    Hovey
    Washburn
    Oxford
    some seventeen miles
    McPherson
    Tallahatchie
    Oxford
    Mississippi
    <class 'str'>
    Sherman
    December
    Memphis
    <class 'str'>
    Army Corps
    Tennessee
    <class 'str'>
    OXFORD
    MISSISSIPPI
    December 8,1862
    W. T. SHERMAN
    Memphis
    <class 'str'>
    Tennessee
    <class 'str'>
    one
    Memphis
    <class 'str'>
    the Mississippi River
    <class 'str'>
    Vicksburg
    <class 'str'>
    Flag
    Porter
    St. Louis
    <class 'str'>
    30,000
    Memphis
    <class 'str'>
    Memphis
    <class 'str'>
    Porter
    four
    U. S. GRANT
    December
    Halleck
    Yallabusha
    <class 'str'>
    Helena
    <class 'str'>
    Memphis
    <class 'str'>
    Vicksburg
    <class 'str'>
    5th
    Oxford
    Halleck
    Helena
    Memphis
    <class 'str'>
    the Yazoo River
    <class 'str'>
    Vicksburg
    State
    Mississippi
    <class 'str'>
    Halleck
    <class 'str'>
    the same day
    5th
    December
    25,000
    Memphis
    <class 'str'>
    the 20th
    Vicksburg
    Sherman
    two
    Sherman
    Sherman
    Halleck
    Sherman
    McClernand
    War
    McClernand
    Sherman
    Halleck
    Yallabusha
    <class 'str'>
    Pemberton
    Vicksburg
    <class 'str'>
    West Tennessee
    <class 'str'>
    Kentucky
    <class 'str'>
    Pemberton
    Sherman
    Vicksburg
    <class 'str'>
    Sherman
    Pemberton
    Vicksburg
    <class 'str'>
    Yallabusha
    <class 'str'>
    Yazoo
    <class 'str'>
    Mississippi
    <class 'str'>
    Sherman
    Vicksburg
    <class 'str'>
    Grenada
    <class 'str'>
    Yallabusha
    <class 'str'>
    Grenada
    <class 'str'>
    Oxford
    seventeen miles
    the 18th of December
    Washington
    <class 'str'>
    four
    McClernand
    one
    Mississippi
    <class 'str'>
    McClernand
    Springfield
    <class 'str'>
    Illinois
    <class 'str'>
    the
    same day
    the 20th
    Van Dorn
    Holly Springs
    <class 'str'>
    secondary
    1,500
    Murphy
    8th
    Wisconsin
    <class 'str'>
    Forrest
    Jackson
    <class 'str'>
    Tennessee
    <class 'str'>
    Columbus
    <class 'str'>
    Kentucky
    <class 'str'>
    more than a week
    more than two weeks
    Columbus
    <class 'str'>
    La Grange
    Memphis
    <class 'str'>
    Mississippi
    <class 'str'>
    Pemberton
    Van Dorn's
    Van Dorn
    Holly Springs
    <class 'str'>
    Murphy
    Van Dorn's
    Murphy
    two months before
    Iuka
    Rosecrans
    one-tenth
    Rosecrans
    Murphy
    Iuka
    <class 'str'>
    Holly Springs
    <class 'str'>
    Murphy
    Pemberton
    Tallahatchie
    Mississippi
    <class 'str'>
    Pemberton
    Van Dorn
    fifteen miles
    two months'
    two months
    two weeks
    twenty days
    only five days'
    Holly Springs
    <class 'str'>
    Holly Springs
    <class 'str'>
    Oxford
    fifteen miles
    fifteen miles
    Sherman
    Memphis
    <class 'str'>
    McClernand
    18th
    McClernand
    Pemberton
    Vicksburg
    <class 'str'>
    the Yazoo River
    <class 'str'>
    Sherman
    one-fourth
    Sherman
    the 20th
    Memphis
    <class 'str'>
    Grenada
    <class 'str'>
    Holly Springs
    <class 'str'>
    Holly Springs
    <class 'str'>
    Van Dorn
    Memphis
    <class 'str'>
    Holly Springs
    <class 'str'>
    the 10th of January
    Holly Springs
    <class 'str'>
    Grand Junction
    Memphis
    <class 'str'>
    Holly Springs
    <class 'str'>
    Sherman
    20,000
    Memphis
    <class 'str'>
    12,000
    Helena
    <class 'str'>
    Arkansas
    <class 'str'>
    the west bank
    <class 'str'>
    McClernand
    2d
    January
    Sherman
    the 13th
    Sherman
    the 15th
    Porter
    navy
    Mississippi
    <class 'str'>
    the Arkansas River
    <class 'str'>
    Arkansas Post
    about fifty miles
    about five or
    six thousand
    Sherman
    McClernand
    Sherman
    three days'
    navy
    5,000
    17
    first
    Five thousand
    Confederate
    Mississippi
    <class 'str'>
    Arkansas Post
    McClernand
    Napoleon
    the Arkansas River
    <class 'str'>
    Sherman
    Porter
    McClernand
    the 17th
    McClernand
    Napoleon
    navy
    McClernand
    McClernand
    McClernand
    Sherman
    December
    McClernand
    Sherman
    the 20th
    McClernand
    Young's Point and Milliken's Bend
    Memphis
    <class 'str'>
    General Hurlbut
    16th
    Memphis
    <class 'str'>
    Charleston
    <class 'str'>
    the Mississippi Central
    Columbus
    Cairo
    <class 'str'>
    Memphis
    <class 'str'>
    the 29th of January
    Young's Point
    the following day
    McClernand
    McClernand
    State
    Congress
    Congress
    Vicksburg
    <class 'str'>
    The Mississippi River
    <class 'str'>
    Cairo
    <class 'str'>
    width
    <class 'str'>
    eighty
    two or more hundred feet
    Memphis
    Vicksburg
    <class 'str'>
    Memphis
    <class 'str'>
    Yallabusha
    <class 'str'>
    Jackson
    <class 'str'>
    Mississippi
    <class 'str'>
    North
    <class 'str'>
    1862
    North
    <class 'str'>
    Vicksburg
    <class 'str'>
    Memphis
    <class 'str'>
    Young's Point
    The winter of 1862-3
    Mississippi
    <class 'str'>
    many miles
    the 17th
    McPherson
    Lake Providence
    seventy miles
    Vicksburg
    <class 'str'>
    January
    the end of
    March
    North
    <class 'str'>
    South
    <class 'str'>
    first
    Memphis
    <class 'str'>
    the Mississippi River
    <class 'str'>
    Mississippi
    <class 'str'>
    Warrenton
    <class 'str'>
    six miles
    Yazoo River
    <class 'str'>
    Haines
    Bluff
    Vicksburg
    <class 'str'>
    Mississippi
    <class 'str'>
    Haines' Bluff
    eleven miles
    Vicksburg
    <class 'str'>
    the Yazoo River
    <class 'str'>
    Vicksburg
    <class 'str'>
    Warrenton
    Young's Point
    Mississippi
    <class 'str'>
    six miles
    Mississippi
    <class 'str'>
    1862
    Thomas Williams
    New Orleans
    <class 'str'>
    ten or twelve feet
    Young's Point
    Williams
    Lincoln
    Mississippi
    <class 'str'>
    McClernand
    Young's Point
    about 4,000
    the 8th of March
    east bank
    <class 'str'>
    two
    thousands
    the 30th of January
    the day
    McPherson
    Lake
    Providence
    the Mississippi River
    <class 'str'>
    the Red River
    <class 'str'>
    Port Hudson
    <class 'str'>
    four hundred miles
    Lake Providence
    Mississippi
    <class 'str'>
    about a mile
    six miles
    Bayou Baxter
    Bayou Macon
    Tensas
    <class 'str'>
    Washita
    <class 'str'>
    Red Rivers
    three
    Bayous Baxter
    Macon
    <class 'str'>
    years
    the Mississippi River
    <class 'str'>
    Memphis
    <class 'str'>
    Bayou Baxter
    Macon
    <class 'str'>
    about two feet
    4th
    McPherson
    several days
    Lake Providence
    Mississippi
    <class 'str'>
    about four hundred
    seventy miles
    Port Hudson
    the Red River
    <class 'str'>
    Mississippi
    <class 'str'>
    Vicksburg
    <class 'str'>
    Washita
    <class 'str'>
    Tensas
    <class 'str'>
    Wilson
    Helena
    Arkansas
    <class 'str'>
    Moon Lake
    <class 'str'>
    the Yazoo
    Pass
    the Mississippi River
    <class 'str'>
    Moon Lake
    <class 'str'>
    a mile
    Coldwater
    Tallahatchie
    Yallabusha
    <class 'str'>
    about two
    hundred
    fifty miles
    Moon Lake
    the Yazoo River
    <class 'str'>
    the State of Mississippi
    some years before
    several hundreds of miles
    2d
    February
    the Mississippi River
    <class 'str'>
    a few miles
    Helena
    Ross
    about 4,500
    Yazoo Pass
    Coldwater
    the 11th of March Ross
    two
    Watson Smith
    Greenwood
    Tallahatchie
    Yallabusha
    <class 'str'>
    Fort
    Pemberton
    Vicksburg
    <class 'str'>
    11th
    the 13th of March
    One
    six
    twenty-five
    Fort Pemberton
    two feet
    second
    Mississippi
    <class 'str'>
    Helena
    six miles
    Ross
    Quinby
    <class 'str'>
    Ross
    Fort Pemberton
    Ross
    Quinby
    <class 'str'>
    another quarter
    Fort Pemberton
    the Yazoo River
    <class 'str'>
    Haines' Bluff
    one
    mile
    the Mississippi at Eagle Bend
    thirty miles
    Young's
    Point.
    Black Bayou
    Black Bayou
    <class 'str'>
    Deer Creek
    <class 'str'>
    Rolling Fork
    Rolling Fork
    the Big
    Sunflower River
    the Big Sunflower
    the Yazoo River
    <class 'str'>
    ten miles
    Haines' Bluff
    twenty
    twenty-five miles
    Porter
    Deer Creek
    the next day
    five
    four
    Sherman
    the 16th
    Stuart
    15th
    Eagle Bend
    Mississippi
    <class 'str'>
    Steel's Bayou
    Porter
    a few hundred
    yards
    about 4,000
    Sherman
    Black Bayou
    the night of the 19th
    Sherman
    Black Bayou
    Black Bayou
    a
    mile
    morning
    twenty-one miles
    noon
    the next day
    Porter
    Mississippi
    <class 'str'>
    fourth
    Vicksburg
    <class 'str'>
    the 27th of
    March
    Lake Providence
    Milliken's Bend
    Young's Point
    Richmond
    <class 'str'>
    Louisiana
    <class 'str'>
    the Mississippi at Carthage
    twenty-five
    thirty miles
    Grand Gulf
    Mississippi
    <class 'str'>
    a few days
    4th
    Halleck
    <class 'str'>
    Lake Providence
    several miles
    Richmond
    <class 'str'>
    Louisiana
    <class 'str'>
    One
    first
    winter
    one
    Vicksburg
    <class 'str'>
    December
    1862
    April
    South
    <class 'str'>
    Measles
    McClernand
    Fremont
    <class 'str'>
    Hunter
    McClellan
    One
    Cairo
    <class 'str'>
    the Army of the Potomac
    one
    the Army of the Potomac
    Hillyer
    first
    Constitution
    Army
    Navy
    Lincoln
    Halleck
    Lincoln
    Milliken
    Porter
    first
    navy
    Porter
    Vicksburg
    navy
    about fourteen miles
    Porter
    one day
    Vicksburg
    <class 'str'>
    Jacob Thompson
    Interior
    Buchanan
    half an
    hour
    Vicksburg
    <class 'str'>
    Thompson
    Porter
    St. Louis
    <class 'str'>
    Chicago
    <class 'str'>
    the
    16th of April
    Porter
    Benton
    Porter
    ten o'clock
    night
    a few minutes
    Louisville
    <class 'str'>
    Mound City
    <class 'str'>
    Pittsburgh
    <class 'str'>
    Carondelet
    Forest
    Queen, Silver Wave
    Henry Clay
    Tuscumbia
    <class 'str'>
    Vicksburg
    Warrenton
    more than two hours
    Henry Clay
    one
    Louisiana
    <class 'str'>
    Porter
    Farragut
    Port Hudson
    Hartford
    <class 'str'>
    one
    Vicksburg
    <class 'str'>
    The 13th
    February
    Porter
    Indianola
    George
    Brown
    Ellet
    Marine
    Natchez
    Two
    Colonel
    Mississippi
    <class 'str'>
    Vicksburg
    <class 'str'>
    the Red River
    <class 'str'>
    Ellet
    Confederate
    the Red River
    <class 'str'>
    two days
    one
    Twenty
    the New Era
    Grand Gulf
    Vicksburg
    <class 'str'>
    Indianola
    Red River
    <class 'str'>
    some
    days
    Mississippi
    <class 'str'>
    Confederates
    One
    Ellet
    February
    2d
    the Red
    River
    <class 'str'>
    Webb
    the Red River
    <class 'str'>
    two
    Indianola
    Mississippi
    <class 'str'>
    Confederate
    Grand Gulf
    the 24th of
    February
    Indianola
    an hour and a half
    seven
    eight
    McClernand
    four
    the
    29th of March
    Richmond
    <class 'str'>
    Louisiana
    <class 'str'>
    New Carthage
    <class 'str'>
    Grand Gulf
    <class 'str'>
    New Carthage
    <class 'str'>
    Bayou Vidal
    two miles
    6th
    McClernand
    New Carthage
    one
    the 17th
    a few days
    McClernand
    Smith's
    eight to twelve miles
    New Carthage
    <class 'str'>
    Milliken's Bend
    twenty-seven
    nearly forty miles
    Four
    two
    six
    hundred feet
    about two thousand feet
    Yankee
    one
    thirty-two
    McClernand
    Lieutenant Hains
    the Engineer
    Corps
    Milliken's Bend on
    the 18th or 19th
    20th
    HEADQUARTERS DEPARTMENT OF THE TENNESSEE
    MILLIKEN'S
    BEND
    <class 'str'>
    LOUISIANA
    <class 'str'>
    April 20, 1863
    110
    the "Army
    Field
    <class 'str'>
    the Mississippi River
    <class 'str'>
    Vicksburg
    <class 'str'>
    Thirteenth
    Major-General
    John A.
    McClernand
    Second.—The Fifteenth
    Major-General
    W. T.
    Sherman
    Seventeenth
    Major-General
    James B.
    McPherson
    march
    New Carthage
    <class 'str'>
    one
    one
    one
    one
    General Orders
    160
    A. G. O.
    1862
    Eighth.—All
    three
    Ninth.—As
    <class 'str'>
    Thirteenth
    Seventeenth
    army corps
    Fifteenth
    Richmond
    <class 'str'>
    Duckport
    Milliken's Bend
    Duckport
    <class 'str'>
    Milliken's Bend
    Milliken's Bend
    ten days'
    one-half
    J. C. Sullivan
    New Carthage
    <class 'str'>
    General Orders
    69
    Adjutant-General's Office
    Washington
    <class 'str'>
    March 20, 1863
    McClernand
    Mississippi
    <class 'str'>
    Two
    McPherson
    third
    Lake Providence
    Milliken's Bend
    Sherman
    McPherson
    Two
    Duckport and Young's Point
    third
    Steele
    Greenville
    <class 'str'>
    Mississippi
    <class 'str'>
    Milliken's Bend and Perkins'
    six
    twelve
    the night of the 22d
    five
    one
    About half
    first
    Vicksburg
    <class 'str'>
    two
    one
    five
    Logan
    Illinois
    <class 'str'>
    Missouri
    <class 'str'>
    two
    W. S. Oliver
    the 24th
    Perkins'
    Grand Gulf
    Hard Times
    twenty-two miles
    Grand Gulf
    two
    six
    only
    10,000
    one
    Lake St. Joseph
    Three
    Richard Yates
    Illinois
    <class 'str'>
    State
    27th
    McClernand
    Hard Times
    McPherson
    the morning of the 29th
    McClernand
    About 10,000
    Grand
    Gulf
    LA
    <class 'str'>
    April 27,1863
    J. A. MCCLERNAND
    13th A. C.
    Grand Gulf
    McPherson
    one
    first
    first
    Porter
    a few days ago
    Grand Gulf
    <class 'str'>
    Rodney
    Grand Gulf
    three
    Grand Gulf
    three days'
    U. S. GRANT
    8 o'clock
    29th
    Porter
    eight
    nearly five and a half hours
    McClernand
    10,000
    About
    half-past
    Admiral
    navy
    eighteen
    fifty-six
    Grand Gulf
    Vicksburg
    <class 'str'>
    Porter
    Louisiana
    <class 'str'>
    Grand Gulf
    about three miles
    Rodney
    <class 'str'>
    Porter
    dusk
    Grand Gulf
    McClernand
    the west bank
    <class 'str'>
    night
    three miles
    the evening of
    29th
    Rodney
    <class 'str'>
    about nine miles
    that night
    Bruinsburg
    <class 'str'>
    Rodney
    Port Gibson
    twelve miles
    Sherman
    Vicksburg
    <class 'str'>
    the
    morning of the 27th
    Yazoo
    <class 'str'>
    Haines
    Pemberton
    Vicksburg
    <class 'str'>
    Sherman
    the day
    Grand Gulf
    <class 'str'>
    the 29th
    ten
    eight
    Porter
    Vicksburg
    <class 'str'>
    Haines'
    Bluff
    first
    May Sherman
    Hard Times
    the evening of
    Haines' Bluff
    McPherson
    two
    Grand Gulf
    the Big Black
    <class 'str'>
    McPherson
    The night of the 29th
    Perkins
    Grand Gulf
    De Shroon's
    Louisiana
    <class 'str'>
    six miles
    Bruinsburg
    <class 'str'>
    Mississippi
    <class 'str'>
    the
    morning
    30th of April
    McClernand
    one
    McPherson
    the month of
    December
    one
    the 13th
    McClernand
    two
    Logan
    17th
    McPherson
    more than twenty thousand
    Logan
    Crocker
    17th
    7th
    two
    15th
    about thirty-three thousand
    Grand Gulf
    Jackson
    nearly sixty thousand
    Jackson
    fifty miles
    first
    Grand Gulf
    <class 'str'>
    Bruinsburg
    <class 'str'>
    two miles
    Mississippi
    <class 'str'>
    Grand Gulf
    <class 'str'>
    Bayou Pierre
    Mississippi
    <class 'str'>
    Bruinsburg
    <class 'str'>
    Port Gibson
    Grand Gulf
    Bruinsburg
    <class 'str'>
    one
    two
    13th
    one
    the
    17th
    the day
    April 30th
    early evening
    McClernand
    two days'
    last five
    McClernand
    Port Gibson
    Bayou Pierre
    Port Gibson
    Grand Gulf
    <class 'str'>
    Vicksburg
    <class 'str'>
    Jackson
    McClernand
    about five miles
    Port
    Gibson
    Thompson
    the
    night
    Grand Gulf
    <class 'str'>
    about seven or eight thousand
    Bowen
    Loring
    Vicksburg
    <class 'str'>
    Loring
    Port
    Gibson
    Two
    McPherson
    McClernand
    the 13th
    Mississippi
    <class 'str'>
    one
    Bowen
    Port
    Gibson
    two
    two
    McClernand
    One
    McClernand
    Hovey, Carr and A. J. Smith
    Osterhaus
    <class 'str'>
    ten
    Osterhaus
    <class 'str'>
    McClernand
    McPherson
    the 13th
    two
    Logan
    about noon
    one brigade
    John E. Smith's
    Osterhaus
    <class 'str'>
    third
    Smith
    Osterhaus
    <class 'str'>
    McClernand
    flank
    night
    about two
    Port Gibson
    <class 'str'>
    the
    night
    next morning
    Port Gibson
    <class 'str'>
    the South
    Fork of the Bayou Pierre
    <class 'str'>
    J. H. Wilson
    one
    eight miles
    the North Fork
    <class 'str'>
    One
    Logan
    Two
    the North Fork
    <class 'str'>
    Port Gibson
    Crocker
    McPherson
    Mississippi
    <class 'str'>
    Bruinsburg
    <class 'str'>
    two days'
    McPherson
    one
    the Mississippi
    River
    Milliken's Bend
    Sherman
    Bruinsburg
    <class 'str'>
    Frederick
    a few weeks
    Grand Gulf
    Thompson
    Hill
    the Battle of Port Gibson
    Grand Gulf
    <class 'str'>
    C. A.
    
    Dana
    the War Department
    Vicksburg
    Fred
    first
    two
    a few days later
    more mature years
    Bruinsburg
    <class 'str'>
    Mississippi
    <class 'str'>
    Milliken's
    some days
    Port Gibson
    <class 'str'>
    A. J. Smith
    Bruinsburg
    <class 'str'>
    nearly a week
    the 30th
    Port Gibson
    first
    Southern
    Grierson
    Mississippi
    <class 'str'>
    La Grange
    April
    17th
    three
    about 1,700
    the 21st
    one
    Columbus
    <class 'str'>
    Macon
    <class 'str'>
    La Grange
    <class 'str'>
    Columbus
    <class 'str'>
    Okalona
    Tupelo
    La
    Grange
    <class 'str'>
    April 26
    Grierson
    about 1,000
    Vicksburg and Meridian
    New
    Orleans
    <class 'str'>
    Jackson
    Baton Rouge
    May 2d
    Grierson
    the night
    2d
    May
    the North Fork
    <class 'str'>
    five
    the next
    morning
    Grand Gulf to Vicksburg
    Grindstone
    Hankinson
    the Big Black
    McPherson
    Hankinson
    before night
    several miles
    Vicksburg
    <class 'str'>
    Vicksburg
    <class 'str'>
    Grand Gulf
    <class 'str'>
    Raymond
    <class 'str'>
    Jackson
    Logan
    Grand Gulf
    McPherson
    Port Gibson
    <class 'str'>
    Logan
    McPherson
    Hankinson
    Willow Springs
    one
    McClernand
    Grand Gulf
    <class 'str'>
    Vicksburg
    <class 'str'>
    six or seven miles
    Vicksburg
    <class 'str'>
    Logan
    the night
    about twenty
    Porter
    Grand Gulf
    May 3d
    the 27th of April
    first
    Cairo
    <class 'str'>
    Sullivan
    Vicksburg
    <class 'str'>
    About twelve o'clock
    Hankinson
    Grand Gulf
    Banks
    Red River
    <class 'str'>
    Port Hudson
    before
    the 10th of May
    only 15,000
    Grand Gulf
    <class 'str'>
    McClernand
    Port Hudson
    <class 'str'>
    Banks
    one
    at least a month
    ten thousand
    three
    hundred miles
    Banks
    Vicksburg
    <class 'str'>
    Grand Gulf
    Washington
    <class 'str'>
    Halleck
    Washington
    <class 'str'>
    Sherman
    four
    Hankinson
    Grand Gulf
    <class 'str'>
    Bruinsburg
    <class 'str'>
    some days
    McClernand
    McPherson
    the night
    2d
    three days'
    mule power
    the night
    Vicksburg
    <class 'str'>
    McClernand
    McPherson
    the Big
    Black
    6th
    Grand Gulf
    the next day
    Three days'
    Grand Gulf
    the next day
    Sherman
    Blair
    Milliken's Bend to Hard Times
    two
    Young's Point
    two
    hundred
    Blair
    one hundred thousand pounds
    3d
    Hurlbut
    Memphis
    <class 'str'>
    four
    Milliken's Bend
    Blair
    5th
    Lauman
    four
    the night of the
    6th
    McPherson
    the Big Black
    an early hour
    Jackson
    Rocky Springs
    <class 'str'>
    Utica
    <class 'str'>
    Raymond
    <class 'str'>
    That night
    McClernand
    Rocky Springs
    ten miles
    Hankinson
    McPherson
    the 8th
    McClernand
    Sherman
    Grand Gulf
    <class 'str'>
    Hankinson
    9th
    McPherson
    a few
    miles
    Utica
    <class 'str'>
    McClernand
    Sherman
    10th
    McPherson
    Utica
    <class 'str'>
    Sherman
    McClernand
    11th
    McClernand
    Five
    Mile Creek
    Sherman
    five miles
    Utica
    <class 'str'>
    May 12th
    McClernand
    Fourteen Mile Creek
    Sherman
    McPherson
    Raymond
    <class 'str'>
    McPherson
    the Big Black
    Pemberton
    the Big Black
    Vicksburg
    <class 'str'>
    McPherson
    Jackson
    two
    six to ten miles
    McClernand
    Fourteen Mile Creek
    McClernand
    Sherman
    McPherson
    Sherman
    Raymond
    <class 'str'>
    one
    Pemberton
    about eighteen thousand
    Haines' Bluff
    Jackson
    Pemberton
    one
    Pemberton
    Jackson
    about seven miles
    eighteen miles
    Jackson
    McPherson
    Sherman
    Fourteen Mile Creek
    McClernand
    Fourteen Mile Creek
    two miles
    Edward
    McClernand
    the Big Black
    McPherson
    five thousand
    two
    Gregg
    about two miles
    Raymond
    <class 'str'>
    about two
    P.M. Logan
    one
    McPherson
    Logan
    Crocker
    Logan
    Crocker
    Gregg
    Jackson
    McPherson
    66
    339
    37
    Logan
    100
    305
    415
    Logan
    Crocker
    Crocker
    McPherson
    Raymond
    Sherman
    Jackson
    Pemberton
    about 18,000
    nearly 50,000
    Jackson
    Vicksburg
    first
    Jackson
    Pemberton
    Jackson
    Pemberton
    the day
    the 13th
    McPherson
    Clinton
    ten miles
    Jackson
    Sherman
    Jackson
    four
    the
    morning
    Raymond
    <class 'str'>
    McClernand
    three
    Dillon
    Raymond
    <class 'str'>
    the Big Black
    <class 'str'>
    10th
    Banks
    the Red
    River
    <class 'str'>
    Porter
    3d
    Port Hudson
    Joseph E. Johnston
    Jackson
    <class 'str'>
    the night
    Tennessee
    <class 'str'>
    Confederate
    Mississippi
    <class 'str'>
    6th
    Halleck
    Tullahoma
    <class 'str'>
    McPherson
    Clinton
    the 13th
    Sherman
    Raymond
    McPherson
    McClernand
    Edward
    the night
    the night of the 13th
    McPherson
    Jackson
    fifteen miles
    Sherman
    Raymond
    <class 'str'>
    Jackson
    McPherson
    two miles
    McClernand
    one
    Clinton
    one
    a few miles
    Mississippi Springs
    <class 'str'>
    Sherman
    third
    Raymond
    four
    Mississippi Springs
    <class 'str'>
    McClernand
    one
    Clinton
    McPherson
    Jackson
    Mississippi
    <class 'str'>
    Sherman
    Raymond
    two
    Blair
    a day
    Jackson
    Jackson
    one day's
    Vicksburg
    <class 'str'>
    three
    Pemberton
    Johnston
    Halleck
    State
    the 14th
    Grand Gulf
    Sherman
    McPherson
    the
    night
    Jackson
    about the same hour
    the night of the 13th
    the day of the 14th
    Sherman
    nine o'clock Crocker
    McPherson
    Raymond
    Johnston
    the night
    Georgia
    <class 'str'>
    South Carolina
    <class 'str'>
    eleven
    thousand
    Sherman
    Jackson
    Confederates
    McPherson
    nearly two miles
    Vicksburg
    <class 'str'>
    McPherson
    Logan
    Crocker
    eleven A.M.
    Crocker
    more than two miles
    McPherson
    about noon
    Sherman
    Mississippi Springs
    <class 'str'>
    west
    <class 'str'>
    the Pearl River
    <class 'str'>
    Sherman
    Sherman
    the Pearl River
    <class 'str'>
    Tuttle
    Tuttle
    McPherson
    Johnston
    <class 'str'>
    Tuttle
    Sherman
    ten
    the State House
    Sherman
    McPherson
    Crocker
    seven
    National
    Mississippi
    <class 'str'>
    Stevenson
    McPherson
    37
    228
    Sherman
    4
    21
    845
    Seventeen
    this day
    Blair
    New Auburn
    <class 'str'>
    McClernand
    4th
    two hundred
    that night
    Johnston
    About four
    Sherman
    Jackson
    <class 'str'>
    Sherman
    Yankee
    C. S. A."
    Sherman
    a few minutes
    Washington
    <class 'str'>
    Congress
    the night of the 13th
    Johnston
    Pemberton
    Edward's
    Sherman
    four
    Clinton
    Time
    One
    Memphis
    <class 'str'>
    some months
    Hurlbut
    Hurlbut
    Johnston
    McPherson
    McPherson
    the morning
    Bolton
    Johnston
    Bolton
    about twenty miles
    Jackson
    McClernand
    Jackson
    the Big Black
    Bolton
    Blair
    the Big Black
    Bolton
    Smith
    Johnston
    Canton
    <class 'str'>
    only six miles
    Jackson
    the night of the 14th
    Pemberton
    Jackson
    Mississippi
    <class 'str'>
    McPherson
    McClernand
    one
    Hovey
    McPherson
    four miles
    One
    Raymond
    <class 'str'>
    Champion
    Carr
    Osterhaus
    <class 'str'>
    Mississippi Springs
    <class 'str'>
    fourth
    Smith
    Blair
    McClernand
    Raymond
    Bolton
    half-past nine
    the
    morning
    The night of the
    15th
    Hovey
    Bolton
    Carr and Osterhaus
    about three miles
    Smith
    Raymond
    <class 'str'>
    Blair
    McPherson
    Logan
    seven
    o'clock
    four
    Hovey
    Crocker
    Hovey
    Clinton
    two
    Jackson
    <class 'str'>
    Clinton
    McClernand
    early in the morning
    Edward
    Pemberton
    Clinton
    Edward
    more than a week before
    15th
    Pemberton
    Edward
    Baker's Creek
    ford
    Jackson
    Baker
    midnight
    16th
    Johnston
    Clinton
    About five o'clock in the morning
    16th
    two
    Jackson
    Vicksburg
    Pemberton
    the
    night
    eighty
    ten
    about
    twenty-five thousand
    Sherman
    Bolton
    one
    an hour
    Steele
    Blair
    Auburn
    Edward
    McClernand
    Blair
    Blair
    15th
    Sherman
    first
    15th
    McPherson
    Hovey
    McClernand
    two
    about three miles
    Edward
    Hovey
    third
    Clinton
    McClernand
    Blair
    A. J. Smith's
    Osterhaus
    <class 'str'>
    Carr
    Smith
    first
    Osterhaus
    <class 'str'>
    Hovey
    Jackson
    <class 'str'>
    Vicksburg
    <class 'str'>
    McPherson
    Hovey
    Hovey
    Clinton
    McPherson
    half-past seven
    Hovey
    McClernand
    McPherson
    McClernand
    Hill
    Pemberton
    first
    Baker's Creek
    Bolton
    Edward
    Baker's Creek
    nearly a mile
    Raymond
    Edward
    three miles
    Champion's Hill
    Bolton
    three and a half miles
    Bolton
    two
    three
    McClernand
    Blair
    McClernand
    Hovey of McClernand's
    McPherson
    Bolton
    Edward
    Baker's Creek
    several miles
    Edward
    Pemberton
    Hovey
    first
    eleven o'clock
    McPherson
    Logan
    Hovey
    Logan
    Hovey
    one
    two
    Crocker
    Hovey
    Crocker
    one
    McPherson
    two
    Logan
    Hovey
    about noon
    Logan
    Baker's Creek
    Hovey
    two
    McPherson's
    Crocker
    two
    McClernand
    two hours
    two miles
    two
    two
    Blair's
    A. J. Smith's
    Ransom
    McArthur
    the
    17th corps
    McPherson
    Grand Gulf
    a few
    days before
    Logan
    Hovey
    McPherson
    Hovey
    Hovey
    Logan
    Crocker
    Crocker
    two
    between three and four
    o'clock
    Carr
    Osterhaus
    <class 'str'>
    Hovey
    McPherson
    two
    Osterhaus
    <class 'str'>
    Carr
    the Big Black
    Osterhaus
    <class 'str'>
    Champion
    Hill
    about four hours
    two or three hours
    Hovey
    McPherson
    two
    Osterhaus's
    A. J. Smith's
    as early as
    McClernand
    two
    a few miles
    noon
    McClernand
    Hovey
    Hovey
    McClernand
    Hovey
    about 15,000
    McClernand
    Hovey
    410
    1,844
    187
    1,200
    more than one-third
    McClernand
    Pemberton
    three thousand
    about three thousand
    Loring
    Pemberton
    Vicksburg
    <class 'str'>
    Pemberton
    that night
    midnight
    1,300
    eleven
    300
    about 700
    500
    1,200
    McPherson
    one
    The night of the
    16th
    May
    McPherson
    two to six miles
    Vicksburg
    <class 'str'>
    Carr
    Osterhaus
    <class 'str'>
    Edward's
    Blair
    about three miles
    Hovey
    thirty
    night
    a
    mile
    ten thousand
    Johnston
    Pemberton
    Pemberton
    a night march
    the Big Black
    Johnston
    Johnston
    Pemberton
    Johnston
    Pemberton
    Sherman
    Jackson
    about noon
    16th
    Bolton
    twenty miles
    two
    Jackson
    Bolton
    early next day
    Bridgeport
    <class 'str'>
    eleven miles
    Blair
    Sherman
    the Big Black
    Sherman
    Carr's
    McClernand
    half-past three
    the 17th
    Osterhaus
    McPherson
    the Big Black
    only six miles
    an early hour
    two
    Carr
    Lawler
    Carr
    McPherson
    General
    Halleck
    11th
    New
    Orleans
    <class 'str'>
    Grand Gulf
    Halleck
    Lawler
    this day
    the west bank
    <class 'str'>
    Eighteen
    1,751
    39
    237
    3
    Vicksburg
    <class 'str'>
    little after nine o'clock
    three
    One
    Lieutenant Hains
    the Engineer Corps
    one
    McPherson
    Ransom
    Hains
    McPherson
    Ransom
    eight o'clock in the morning of the 18th
    three
    Sherman
    Bridgeport
    <class 'str'>
    about noon of
    Blair
    the west bank
    <class 'str'>
    Two
    that night
    third
    the following morning
    the 18th
    Sherman
    first
    the Yazoo River
    <class 'str'>
    Vicksburg
    <class 'str'>
    Sherman
    Walnut Hills
    <class 'str'>
    December
    Sherman
    Haines' Bluff
    Vicksburg
    <class 'str'>
    a few minutes Sherman
    December
    this minute
    Sherman
    McPherson
    the Big Black
    Jackson
    Vicksburg
    Sherman
    night
    McClernand
    Mount
    Albans
    <class 'str'>
    Baldwin
    Vicksburg
    <class 'str'>
    McPherson
    three
    Vicksburg
    <class 'str'>
    three
    one
    the morning
    19th
    Sherman
    Yazoo
    <class 'str'>
    McPherson
    Jackson
    McClernand
    Warrenton
    the 19th
    Champion's Hill
    the Big Black
    Vicksburg
    <class 'str'>
    two o'clock
    The 20th and 21st
    Yazoo River
    <class 'str'>
    Chickasaw
    Bayou
    three weeks
    only five
    
    the 21st
    the night of the 21st
    second
    Johnston
    only fifty miles
    Pemberton
    Vicksburg
    <class 'str'>
    Johnston
    State
    first
    ten o'clock
    A.M.
    the same minute
    three
    McClernand
    Quinby
    17th
    Sherman
    McPherson
    McClernand
    all day
    Vicksburg
    <class 'str'>
    Bruinsburg
    <class 'str'>
    April 30th
    18th
    Vicksburg
    <class 'str'>
    the 19th
    five
    State
    about one hundred
    five days'
    six thousand
    twenty-seven
    sixty-one
    four hundred miles
    Vicksburg
    <class 'str'>
    Port Hudson
    <class 'str'>
    the Mississippi River
    <class 'str'>
    forty-three thousand
    One
    Blair
    Champion
    Hill
    one
    Ransom
    McPherson
    Vicksburg
    <class 'str'>
    Grand Gulf
    <class 'str'>
    Jackson
    sixty thousand
    Port Gibson
    Raymond
    <class 'str'>
    five thousand
    Jackson
    <class 'str'>
    eight
    Champion's Hill
    twenty-five thousand
    the
    Big Black
    four thousand
    Jackson
    Raymond
    <class 'str'>
    half
    Vicksburg
    <class 'str'>
    McPherson
    McArthur
    McClernand
    Warrenton
    Lauman
    the 19th and 22d
    the Yazoo River
    <class 'str'>
    Chickasaw Bayou
    Mississippi
    <class 'str'>
    Hurlbut
    the Big Black
    <class 'str'>
    Johnston
    Johnston
    Bragg
    Rosecrans
    Tennessee
    <class 'str'>
    more than fifteen miles
    Haines'
    Bluff
    Vicksburg
    <class 'str'>
    Warrenton
    about seven
    Canton
    <class 'str'>
    Jackson
    second
    Halleck
    Vicksburg
    <class 'str'>
    about two hundred feet
    the Mississippi River
    <class 'str'>
    Jackson
    three miles
    one
    four
    the Engineer Corps
    Captain Comstock
    the Engineer Corps
    West Point
    <class 'str'>
    Army
    two hundred and twenty pounds
    six thirty-two
    West
    <class 'str'>
    Porter
    first
    one
    six
    twelve
    the 30th
    of June
    two hundred and twenty
    Vicksburg
    <class 'str'>
    Johnston
    Champion
    Vicksburg
    North
    <class 'str'>
    Christian
    a dozen or two
    Illinois
    <class 'str'>
    State
    Sherman
    Sherman
    Sherman
    North
    <class 'str'>
    Sherman
    Sherman
    Walnut Hills
    <class 'str'>
    the 18th of May
    Sherman
    Sherman
    first
    Sherman
    Sherman
    Memphis
    <class 'str'>
    Sherman
    Memphis
    <class 'str'>
    Grenada
    <class 'str'>
    North
    <class 'str'>
    Memphis
    <class 'str'>
    J. A. Rawlins
    Rawlins
    Sherman
    Sherman
    Badeau
    Sherman
    Sherman
    26th
    Blair
    Yazoo
    <class 'str'>
    the Big Black
    Blair
    Blair
    forty-five miles
    almost a week
    Porter
    Haines' Bluff
    Banks
    ten thousand
    Port Hudson
    3d
    June
    Hurlbut
    Kimball
    Mechanicsburg
    <class 'str'>
    Haines' Bluff
    the Big
    Black
    Blair
    twelve
    hundred
    Blair
    Yazoo
    Blair
    the 7th of June
    Mississippi
    <class 'str'>
    Milliken's Bend
    about
    3,000
    Richard Taylor's
    Mower
    the Tensas
    Bayou
    that quarter
    first
    the 8th of June
    Hurlbut's
    Sooy Smith
    Haines' Bluff
    C. C. Washburn
    the 11th
    the Department of the
    
    Missouri
    <class 'str'>
    Herron
    Pemberton
    Johnston
    Lauman
    McClernand
    Herron
    Lauman
    a few hundred yards
    Confederate
    night
    the 14th
    General Parke
    two
    Burnside
    <class 'str'>
    Haines' Bluff
    Herron
    Halleck
    <class 'str'>
    about seventy-one thousand
    More than half
    the Big Black
    Osterhaus
    <class 'str'>
    Jackson
    Baldwin
    eight
    Vicksburg
    <class 'str'>
    the 17th
    Sherman
    one
    18th
    McPherson
    McClernand
    13th
    North
    <class 'str'>
    McClernand
    McClernand
    13th
    army corps
    Springfield
    <class 'str'>
    Illinois
    <class 'str'>
    War Department
    the 22d of June
    Johnston
    the Big Black River
    <class 'str'>
    Pemberton
    Johnston
    Pemberton
    Vicksburg
    Johnston
    Sherman
    Haines' Bluff
    the Big Black River
    <class 'str'>
    Vicksburg
    <class 'str'>
    Herron
    A. J. Smith's
    Sherman
    Haines' Bluff
    the Big Black
    Pemberton
    Johnston
    Vicksburg
    <class 'str'>
    Johnston
    Pemberton
    Johnston
    May
    three
    Jackson
    Leggett
    the 25th of June
    two
    Confederates
    the 25th of June
    three o'clock
    one
    Logan
    two
    the night
    the 1st of July
    redan
    25th
    first
    about thirty
    two
    first
    second
    three
    one
    Johnston
    <class 'str'>
    Pemberton
    Johnston
    Vicksburg
    <class 'str'>
    the 21st of June
    Pemberton
    Louisiana
    <class 'str'>
    night
    the "Yankees
    a week
    Porter
    the west bank
    <class 'str'>
    Louisiana
    <class 'str'>
    Vicksburg
    <class 'str'>
    Louisiana
    <class 'str'>
    Richard Taylor
    the west bank
    <class 'str'>
    Mississippi
    <class 'str'>
    Vicksburg
    <class 'str'>
    Port Hudson
    <class 'str'>
    Lake Providence
    Bruinsburg
    <class 'str'>
    the 1st of July
    ten
    one hundred yards
    6th of July
    four
    the night of the 1st of July
    Brownsville
    <class 'str'>
    the Big Black
    Pemberton
    7th
    Pemberton
    July 1st
    Pemberton
    four
    Vicksburg
    <class 'str'>
    Two
    two
    Pemberton
    Johnston
    Johnston
    Pemberton
    3d
    A.M.
    two
    Bowen
    Pemberton
    Vicksburg
    <class 'str'>
    three
    John S.
    Bowen
    Northern
    Vicksburg
    Bowen
    A. J. Smith
    Bowen
    Missouri
    <class 'str'>
    Pemberton
    Pemberton
    McPherson
    three o'clock that afternoon
    Pemberton
    several hours
    Vicksburg
    <class 'str'>
    three o'clock
    Pemberton
    the morning
    Generals Ord
    McPherson
    Logan
    A. J.
    Smith
    a few hundred feet
    The True Cross
    Pemberton
    the Mexican War
    Pemberton
    Bowen
    Pemberton
    Bowen
    Pemberton
    Bowen
    Confederate
    ten o'clock that night
    Porter
    Pemberton
    navy
    Pemberton
    Vicksburg
    eight to twelve miles
    Johnston
    Pemberton
    this afternoon
    the City of
    Vicksburg
    <class 'str'>
    one
    eight
    morrow
    one
    Thirty
    two
    two
    Aiken
    Dutch
    the James River
    <class 'str'>
    Confederate
    Vicksburg
    <class 'str'>
    thirty thousand
    Cairo
    <class 'str'>
    Mississippi
    <class 'str'>
    Washington
    <class 'str'>
    Baltimore
    <class 'str'>
    Aiken
    Confederates
    Pemberton
    South
    <class 'str'>
    Vicksburg
    <class 'str'>
    ten o'clock
    A.M. to-morrow
    Vicksburg
    <class 'str'>
    garrison
    midnight
    3d July
    last evening
    one
    ten o'clock
    nine
    o'clock
    A.M.
    Pemberton
    two
    Johnnies
    Yanks
    Well, Yank
    4th
    July
    fourth
    Vicksburg
    fourth
    Yankee
    Vicksburg
    <class 'str'>
    that day
    First
    fourth
    Pemberton
    third
    two-fold
    first
    second
    the Declaration of American Independence
    Vicksburg
    <class 'str'>
    Logan
    first
    one
    two
    Pemberton
    4th
    July
    the
    day
    4th
    July
    the
    day
    first
    about 10 o'clock
    July
    3d
    twenty-four
    hours
    Johnston
    fourth
    two weeks
    4th
    Holmes
    eight
    thousand
    Helena
    <class 'str'>
    Arkansas
    <class 'str'>
    Helena
    less than forty-two
    Holmes
    1,636
    173
    Prentiss
    400
    Holmes
    Union
    57
    127
    40
    Vicksburg
    <class 'str'>
    third
    Sherman
    Johnston
    State
    Ord
    Sherman
    Sherman
    Vicksburg
    <class 'str'>
    navy
    Vicksburg
    the Big Black
    a few feet
    two
    the afternoon
    sixth
    the afternoon
    fourth
    Captain Wm
    M. Dunn
    Cairo
    <class 'str'>
    this morning
    several days
    Johnston
    State
    9th
    Burnside
    <class 'str'>
    Gettysburg
    <class 'str'>
    Cabinet
    North
    <class 'str'>
    Vicksburg
    MORALE
    General Banks
    the Mississippi River
    <class 'str'>
    General Banks
    Port Hudson
    <class 'str'>
    National
    Vicksburg
    <class 'str'>
    General Banks
    Vicksburg
    Gardner
    the 9th of July
    Port Hudson
    nearly 6,000
    51
    5,000
    that day
    the Mississippi River
    <class 'str'>
    National
    Pemberton
    Vicksburg
    <class 'str'>
    one
    Several hundred
    North
    <class 'str'>
    Pemberton
    Pemberton
    eleventh
    just one week
    Confederate
    the James River
    <class 'str'>
    two
    The day
    morrow
    Edward's Ferry
    Meant Edward's Station
    Raymond
    Vicksburg
    <class 'str'>
    North
    <class 'str'>
    Gettysburg
    <class 'str'>
    the same day
    the Mississippi River
    <class 'str'>
    National
    Vicksburg
    <class 'str'>
    Port Hudson
    <class 'str'>
    Virginia
    <class 'str'>
    Pennsylvania
    <class 'str'>
    1861
    The Army of the Tennessee
    Army
    Gulf
    <class 'str'>
    Confederate States
    <class 'str'>
    first
    Vicksburg
    <class 'str'>
    Vicksburg
    <class 'str'>
    seventh
    Halleck
    Watts
    <class 'str'>
    Confederate
    Vicksburg 31,600
    172
    about 60,000
    West
    <class 'str'>
    United States
    <class 'str'>
    Belgian
    one
    the
    Ordnance Department
    Vicksburg
    <class 'str'>
    Vicksburg
    Ransom
    Logan
    Crocker
    F. P. Blair
    Milliken's Bend
    Blair
    Missouri
    <class 'str'>
    1858
    Congress
    two
    one
    one
    Porter
    two
    Vicksburg
    <class 'str'>
    1862
    New Orleans
    <class 'str'>
    Grand Gulf
    Vicksburg
    <class 'str'>
    Grand Gulf
    Port Hudson
    under ten days
    only fifteen
    thousand
    Jackson
    the
    day
    only a few days
    Vicksburg
    <class 'str'>
    two
    Providence
    the Army of the Tennessee
    Vicksburg
    <class 'str'>
    three
    first
    State
    second
    Banks
    Port Hudson
    <class 'str'>
    Mississippi
    <class 'str'>
    Stars and Stripes
    third
    Washington
    <class 'str'>
    North
    <class 'str'>
    Pemberton
    Sherman
    Haines' Bluff
    Vicksburg
    <class 'str'>
    Jackson
    the Big Black
    <class 'str'>
    State
    Vicksburg
    Ord
    Sherman
    Johnston
    Sherman
    Sherman
    the Big Black
    <class 'str'>
    three
    Bolton
    twenty miles
    Jackson
    <class 'str'>
    Johnston
    Jackson
    the 8th of
    July
    ten miles
    Jackson
    11th
    the morning of the 17th
    the night
    Johnston
    Sherman
    one
    Steele
    Brandon
    <class 'str'>
    fourteen miles
    Jackson
    <class 'str'>
    National
    second
    Jackson
    thousand
    Confederate
    Confederate
    Jackson
    <class 'str'>
    Raymond
    Sherman
    State
    Bruinsburg
    <class 'str'>
    Jackson
    <class 'str'>
    Vicksburg
    <class 'str'>
    Sherman
    Vicksburg
    <class 'str'>
    the Big Black
    Haines' Bluff
    Vicksburg
    <class 'str'>
    Confederate
    more than a hundred miles
    Mobile
    <class 'str'>
    Lake
    <class 'str'>
    Halleck
    Mississippi
    <class 'str'>
    Texas
    <class 'str'>
    Confederate States
    <class 'str'>
    Louisiana
    <class 'str'>
    Texas
    <class 'str'>
    Brownsville
    <class 'str'>
    the Rio Grande
    Halleck
    Mobile
    <class 'str'>
    a year before
    west Tennessee
    <class 'str'>
    Mobile
    General Bragg's
    Bragg
    Mobile
    Lee
    July
    1st
    August
    Mobile
    <class 'str'>
    New
    Orleans
    <class 'str'>
    Mobile
    Halleck
    the year before
    Corinth
    4,000
    9th
    Kentucky
    <class 'str'>
    5,000
    Schofield
    <class 'str'>
    Missouri
    <class 'str'>
    Price
    State
    Ransom
    Natchez
    Ransom
    about 5,000
    Texas
    <class 'str'>
    Eastern
    Texas
    <class 'str'>
    the Rio Grande
    Lee
    East
    <class 'str'>
    Vicksburg
    <class 'str'>
    first
    Vicksburg
    <class 'str'>
    Pemberton
    Johnston
    Confederate
    North
    <class 'str'>
    Mississippi
    <class 'str'>
    Pemberton
    4,000
    the 7th of August
    the
    13th
    Ord
    Mississippi
    <class 'str'>
    New
    Orleans
    <class 'str'>
    Carrollton
    New Orleans
    <class 'str'>
    Vicksburg
    <class 'str'>
    Sherman
    Sherman
    the 13th of September
    New Orleans
    <class 'str'>
    Halleck
    Memphis
    <class 'str'>
    Tuscumbia
    <class 'str'>
    Rosecrans
    Chattanooga
    <class 'str'>
    the 15th
    Rosecrans
    the 27th
    Sherman
    one
    Memphis
    <class 'str'>
    McPherson
    Steele
    Arkansas
    <class 'str'>
    Memphis
    <class 'str'>
    Hurlbut
    two
    two
    Halleck
    Sherman
    McPherson
    Memphis
    <class 'str'>
    Sherman
    one
    one
    McPherson
    Chickamauga
    Rosecrans
    Chattanooga
    Charles A.
    Dana
    the War Department
    Rosecrans
    Chattanooga
    Halleck
    Nashville
    <class 'str'>
    October 3d
    War
    Grant
    Cairo
    <class 'str'>
    Arriving
    Columbus
    <class 'str'>
    Cairo
    <class 'str'>
    3d
    Cairo
    <class 'str'>
    11.30
    the 10th
    the same day
    Cairo
    <class 'str'>



```python
# testing efficacy of dictionary
place_freq
```




    {'East': 8,
     'Pacific': 4,
     'West': 9,
     'Panama': 1,
     'St. Louis': 33,
     'Ohio': 23,
     'Boggs': 1,
     'Illinois': 27,
     'Missouri': 35,
     'Slave States': 2,
     'Free States': 1,
     'St. Louis City': 1,
     'United States': 7,
     'the United States': 12,
     'North': 33,
     'Texas': 9,
     'South': 27,
     'the Slave States': 2,
     'Chicago': 3,
     'the Southern States': 3,
     'North-west': 1,
     'Southern States': 1,
     'States': 10,
     'new States': 1,
     'Florida': 4,
     'the States': 1,
     'Mississippi': 80,
     'Russia': 1,
     'south-west\r\nWisconsin': 1,
     'Minnesota': 1,
     'north-east Iowa': 1,
     'Fort Donelson': 10,
     'La Grange': 7,
     'The States of Virginia': 1,
     'Kentucky': 25,
     'South Carolina': 3,
     'Maryland': 3,
     'Delaware': 1,
     'Confederate States': 3,
     'Montgomery': 1,
     'Alabama': 3,
     'the\r\nUnion': 2,
     'Fort Sumter': 1,
     'Charleston': 12,
     'the Northern States': 1,
     'Springfield': 13,
     'Belleville': 1,
     'St.\r\nLouis': 1,
     'West Point': 13,
     'Camp Jackson': 1,
     'St. Louis for Mattoon': 1,
     'ILLINOIS': 1,
     'Washington': 22,
     'Indiana': 2,
     'Covington': 1,
     'Cincinnati': 2,
     'Mattoon': 1,
     'Quincy': 4,
     'the Illinois\r\nRiver': 1,
     'the Illinois River': 1,
     'Hannibal': 1,
     'St. Joe': 1,
     'Dubuque': 1,
     'Iowa': 1,
     'Mexico': 8,
     'the Mississippi River': 19,
     'Salt River': 5,
     'Greenville': 5,
     'Jefferson City': 5,
     'Lexington': 3,
     'Jefferson\r\nCity': 2,
     'Cape Girardeau': 8,
     'Jacksonville': 1,
     'Cairo': 45,
     'Belmont': 15,
     'Jackson': 17,
     'Arkansas': 9,
     'New York': 1,
     'Columbus': 48,
     'Paducah': 17,
     'Tennessee': 57,
     'Cumberland': 19,
     'the White River': 1,
     'Fort Holt': 1,
     'the west bank': 15,
     'the east bank': 2,
     'the\r\neast bank': 1,
     'The Mississippi River': 2,
     'west Kentucky': 2,
     'the Cumberland River': 5,
     'Fort Henry': 6,
     'Fort': 2,
     'Nashville': 43,
     'Memphis': 79,
     'the District of South-east': 1,
     'District': 1,
     'Louisville': 7,
     'the Tennessee River': 6,
     'Fort Heiman': 2,
     'Dover': 12,
     'west bank': 1,
     'Heiman': 1,
     'Kansas': 2,
     'Nebraska': 1,
     'Alps': 1,
     'Pittsburg': 39,
     'Conestoga': 1,
     'Richmond': 10,
     'Mound City': 3,
     'DOVER': 1,
     'TENNESSEE': 1,
     'Confederates': 1,
     'Smithland': 1,
     'Vicksburg': 100,
     'Ohio River': 1,
     'Clarksville': 6,
     'The Cumberland River': 1,
     'Edgefield': 1,
     'Diana': 1,
     'Woodford': 1,
     'Autocrat': 1,
     'the Alleghany Mountains': 1,
     'Mill Springs': 2,
     'the States of Kentucky': 1,
     'Eastport': 3,
     'Paris': 1,
     'Halleck': 7,
     'Savannah': 13,
     'the Mississippi\r\nRiver': 1,
     'west Tennessee': 2,
     'Hamburg': 4,
     'Tennessee River': 1,
     'Columbia': 1,
     'Mobile': 14,
     'Buckland': 1,
     'P.M.': 1,
     'Purdy': 2,
     'Snake Creek': 5,
     'Snake': 1,
     'Lick creeks': 1,
     'Prentiss': 2,
     'Lick\r\nCreek': 1,
     'Lew': 2,
     'The Tennessee River': 1,
     'Shiloh': 2,
     'Adamsville': 1,
     'NEW YORK': 1,
     'Johnston': 4,
     'the Ohio River': 1,
     'the\r\nConfederates': 1,
     'Northern': 2,
     'Hickman': 1,
     'Knoxville': 2,
     'Atlantic': 1,
     'the States of Tennessee': 1,
     'the Tuscumbia River': 1,
     'Farmington': 3,
     'West Tennessee': 4,
     'New\r\nOrleans': 5,
     'Baton Rouge': 2,
     'Chattanooga': 2,
     'Stone River': 1,
     'Atlanta': 1,
     'Manitou Springs': 1,
     'Colorado': 1,
     'West\r\n': 1,
     'Cumberland River': 1,
     'Rienzi': 3,
     'Bolivar': 10,
     'the Hatchie River': 3,
     'Rome': 2,
     'Georgia': 2,
     'Holly Springs': 16,
     'Decatur': 1,
     'Tupelo': 1,
     'Michigan': 2,
     'Booneville': 1,
     'Corinth': 2,
     'Jacinto': 7,
     'Brownsville': 5,
     'Bragg': 1,
     'Middle Tennessee': 3,
     'Alleghanies': 1,
     'Wisconsin': 2,
     'Iuka': 10,
     'Burnsville': 1,
     'Helena': 5,
     'Mississippi Central': 1,
     'Ripley': 1,
     '20th': 1,
     '45th Illinois': 1,
     'Shreveport': 1,
     'Louisiana': 11,
     'Grenada': 5,
     'the Mississippi Central': 1,
     'Bear Creek': 1,
     'Mississippi River': 1,
     'Cottage Hill': 1,
     'Yallabusha': 7,
     'the Yazoo River': 8,
     'Yazoo': 4,
     'the Arkansas River': 2,
     'width': 1,
     'Warrenton': 1,
     'Yazoo River': 2,
     'New Orleans': 4,
     'east bank': 1,
     'the Red River': 5,
     'Port Hudson': 7,
     'Tensas': 2,
     'Washita': 2,
     'Macon': 3,
     'Moon Lake': 2,
     'Quinby': 2,
     'Black Bayou': 1,
     'Deer Creek': 1,
     'Fremont': 1,
     'Pittsburgh': 1,
     'Tuscumbia': 2,
     'Hartford': 1,
     'Red River': 2,
     'the Red\r\nRiver': 2,
     'New Carthage': 5,
     'Grand Gulf': 14,
     'BEND': 1,
     'LOUISIANA': 1,
     'Field': 1,
     'Ninth.—As': 1,
     'Duckport': 1,
     'LA': 1,
     'Rodney': 2,
     'Bruinsburg': 13,
     'the Big Black': 5,
     'Osterhaus': 12,
     'Port Gibson': 4,
     'the South\r\nFork of the Bayou Pierre': 1,
     'the North Fork': 3,
     'La\r\nGrange': 1,
     'Raymond': 12,
     'Rocky Springs': 1,
     'Utica': 4,
     'Tullahoma': 1,
     'Mississippi Springs': 4,
     'west': 1,
     'the Pearl River': 2,
     'New Auburn': 1,
     'Canton': 2,
     'Bridgeport': 2,
     'Walnut Hills': 2,
     'Mount\r\nAlbans': 1,
     'Mechanicsburg': 1,
     'Burnside': 2,
     'the Big Black River': 2,
     'the City of\r\nVicksburg': 1,
     'the James River': 2,
     'Baltimore': 1,
     'Gettysburg': 2,
     'Virginia': 1,
     'Pennsylvania': 1,
     'Gulf': 1,
     'Watts': 1,
     'Brandon': 1,
     'Lake': 1,
     'Schofield': 1}




```python
# creation of dataframe based on locations found, and their corresponding frequency values
place_freq_df = pd.DataFrame.from_dict(place_freq, orient='index').reset_index().rename(columns={'index':'Location',0:'Frequency'})
place_freq_df
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Location</th>
      <th>Frequency</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>East</td>
      <td>8</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Pacific</td>
      <td>4</td>
    </tr>
    <tr>
      <th>2</th>
      <td>West</td>
      <td>9</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Panama</td>
      <td>1</td>
    </tr>
    <tr>
      <th>4</th>
      <td>St. Louis</td>
      <td>33</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>259</th>
      <td>Gulf</td>
      <td>1</td>
    </tr>
    <tr>
      <th>260</th>
      <td>Watts</td>
      <td>1</td>
    </tr>
    <tr>
      <th>261</th>
      <td>Brandon</td>
      <td>1</td>
    </tr>
    <tr>
      <th>262</th>
      <td>Lake</td>
      <td>1</td>
    </tr>
    <tr>
      <th>263</th>
      <td>Schofield</td>
      <td>1</td>
    </tr>
  </tbody>
</table>
<p>264 rows × 2 columns</p>
</div>




```python
# export of dataframe in the form of csv file
place_freq_df.to_csv('grant_places.csv')
```


```python
# accessing Geonames geographical gazetteer database pertaining to locations in the USA
places = pd.read_table('US.txt')
places.columns = ['geonameid', 'name', 'asciiname', 'alternatenames', 'latitude', 'longitude', 'featureclass', 'featurecode', 'countrycode', 'cc2', 'state', 'admin1 code', 'admin2 code', 'admin3 code', 'admin4 code', 'population', 'elevation', 'timezone', 'modification date']
places.columns
```

    /var/folders/3s/__tw7mvj4_dfnkdgp84427k00000gn/T/ipykernel_82803/1626289984.py:2: DtypeWarning: Columns (9,11) have mixed types. Specify dtype option on import or set low_memory=False.
      places = pd.read_table('US.txt')





    Index(['geonameid', 'name', 'asciiname', 'alternatenames', 'latitude',
           'longitude', 'featureclass', 'featurecode', 'countrycode', 'cc2',
           'state', 'admin1 code', 'admin2 code', 'admin3 code', 'admin4 code',
           'population', 'elevation', 'timezone', 'modification date'],
          dtype='object')




```python
# testing efficacy of dataframe
places
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>geonameid</th>
      <th>name</th>
      <th>asciiname</th>
      <th>alternatenames</th>
      <th>latitude</th>
      <th>longitude</th>
      <th>featureclass</th>
      <th>featurecode</th>
      <th>countrycode</th>
      <th>cc2</th>
      <th>state</th>
      <th>admin1 code</th>
      <th>admin2 code</th>
      <th>admin3 code</th>
      <th>admin4 code</th>
      <th>population</th>
      <th>elevation</th>
      <th>timezone</th>
      <th>modification date</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2130833</td>
      <td>McArthur Reef</td>
      <td>McArthur Reef</td>
      <td>NaN</td>
      <td>52.06667</td>
      <td>177.86667</td>
      <td>U</td>
      <td>RFU</td>
      <td>US</td>
      <td>NaN</td>
      <td>AK</td>
      <td>16.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0</td>
      <td>NaN</td>
      <td>-9999</td>
      <td>Asia/Kamchatka</td>
      <td>2016-07-05</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2130890</td>
      <td>Buldir Reef</td>
      <td>Buldir Reef</td>
      <td>NaN</td>
      <td>52.15229</td>
      <td>176.35675</td>
      <td>U</td>
      <td>RFU</td>
      <td>US</td>
      <td>NaN</td>
      <td>AK</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0</td>
      <td>NaN</td>
      <td>-9999</td>
      <td>NaN</td>
      <td>2022-05-25</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3577483</td>
      <td>The Narrows</td>
      <td>The Narrows</td>
      <td>The Narrows</td>
      <td>18.37502</td>
      <td>-64.72517</td>
      <td>H</td>
      <td>CHN</td>
      <td>US</td>
      <td>VG</td>
      <td>00</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0</td>
      <td>NaN</td>
      <td>-9999</td>
      <td>America/St_Thomas</td>
      <td>2018-11-06</td>
    </tr>
    <tr>
      <th>3</th>
      <td>3715657</td>
      <td>Cape Lookout Shoals</td>
      <td>Cape Lookout Shoals</td>
      <td>Lookout Shoals</td>
      <td>34.51206</td>
      <td>-76.49326</td>
      <td>H</td>
      <td>SHOL</td>
      <td>US</td>
      <td>NaN</td>
      <td>NC</td>
      <td>31.0</td>
      <td>91396.0</td>
      <td>NaN</td>
      <td>0</td>
      <td>NaN</td>
      <td>-9999</td>
      <td>America/New_York</td>
      <td>2022-05-24</td>
    </tr>
    <tr>
      <th>4</th>
      <td>3831601</td>
      <td>Nantucket Shoals</td>
      <td>Nantucket Shoals</td>
      <td>NaN</td>
      <td>41.06917</td>
      <td>-69.67983</td>
      <td>U</td>
      <td>SHSU</td>
      <td>US</td>
      <td>NaN</td>
      <td>MA</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0</td>
      <td>NaN</td>
      <td>-9999</td>
      <td>NaN</td>
      <td>2019-03-18</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>2240191</th>
      <td>12503499</td>
      <td>San Vicente Reservoir</td>
      <td>San Vicente Reservoir</td>
      <td>NaN</td>
      <td>32.91438</td>
      <td>-116.93431</td>
      <td>S</td>
      <td>BTYD</td>
      <td>US</td>
      <td>NaN</td>
      <td>CA</td>
      <td>73.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0</td>
      <td>NaN</td>
      <td>268</td>
      <td>America/Los_Angeles</td>
      <td>2022-12-22</td>
    </tr>
    <tr>
      <th>2240192</th>
      <td>12503500</td>
      <td>Boulder Bay</td>
      <td>Boulder Bay</td>
      <td>NaN</td>
      <td>32.91401</td>
      <td>-116.93058</td>
      <td>S</td>
      <td>MAR</td>
      <td>US</td>
      <td>NaN</td>
      <td>CA</td>
      <td>73.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0</td>
      <td>NaN</td>
      <td>195</td>
      <td>America/Los_Angeles</td>
      <td>2022-12-22</td>
    </tr>
    <tr>
      <th>2240193</th>
      <td>12503501</td>
      <td>San Vicente Water Recreation Area</td>
      <td>San Vicente Water Recreation Area</td>
      <td>NaN</td>
      <td>32.92518</td>
      <td>-116.92768</td>
      <td>L</td>
      <td>RGNL</td>
      <td>US</td>
      <td>NaN</td>
      <td>CA</td>
      <td>73.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0</td>
      <td>NaN</td>
      <td>298</td>
      <td>America/Los_Angeles</td>
      <td>2022-12-22</td>
    </tr>
    <tr>
      <th>2240194</th>
      <td>12503502</td>
      <td>El Capitan Dam</td>
      <td>El Capitan Dam</td>
      <td>NaN</td>
      <td>32.88605</td>
      <td>-116.80894</td>
      <td>S</td>
      <td>SPLY</td>
      <td>US</td>
      <td>NaN</td>
      <td>CA</td>
      <td>73.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0</td>
      <td>NaN</td>
      <td>227</td>
      <td>America/Los_Angeles</td>
      <td>2022-12-22</td>
    </tr>
    <tr>
      <th>2240195</th>
      <td>12503503</td>
      <td>El Capitan Water Recreation Area</td>
      <td>El Capitan Water Recreation Area</td>
      <td>NaN</td>
      <td>32.87421</td>
      <td>-116.79520</td>
      <td>L</td>
      <td>PRK</td>
      <td>US</td>
      <td>NaN</td>
      <td>CA</td>
      <td>73.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0</td>
      <td>NaN</td>
      <td>237</td>
      <td>America/Los_Angeles</td>
      <td>2022-12-22</td>
    </tr>
  </tbody>
</table>
<p>2240196 rows × 19 columns</p>
</div>




```python
# modifying visible columns of dataframe
places = places.drop(columns = ['asciiname', 'featureclass', 'cc2', 'admin1 code', 'admin2 code', 'admin3 code', 'admin4 code', 'population', 'elevation', 'timezone', 'modification date'])
```


```python
# testing efficacy of dataframe
places
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>geonameid</th>
      <th>name</th>
      <th>alternatenames</th>
      <th>latitude</th>
      <th>longitude</th>
      <th>featurecode</th>
      <th>countrycode</th>
      <th>state</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2130833</td>
      <td>McArthur Reef</td>
      <td>NaN</td>
      <td>52.06667</td>
      <td>177.86667</td>
      <td>RFU</td>
      <td>US</td>
      <td>AK</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2130890</td>
      <td>Buldir Reef</td>
      <td>NaN</td>
      <td>52.15229</td>
      <td>176.35675</td>
      <td>RFU</td>
      <td>US</td>
      <td>AK</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3577483</td>
      <td>The Narrows</td>
      <td>The Narrows</td>
      <td>18.37502</td>
      <td>-64.72517</td>
      <td>CHN</td>
      <td>US</td>
      <td>00</td>
    </tr>
    <tr>
      <th>3</th>
      <td>3715657</td>
      <td>Cape Lookout Shoals</td>
      <td>Lookout Shoals</td>
      <td>34.51206</td>
      <td>-76.49326</td>
      <td>SHOL</td>
      <td>US</td>
      <td>NC</td>
    </tr>
    <tr>
      <th>4</th>
      <td>3831601</td>
      <td>Nantucket Shoals</td>
      <td>NaN</td>
      <td>41.06917</td>
      <td>-69.67983</td>
      <td>SHSU</td>
      <td>US</td>
      <td>MA</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>2240191</th>
      <td>12503499</td>
      <td>San Vicente Reservoir</td>
      <td>NaN</td>
      <td>32.91438</td>
      <td>-116.93431</td>
      <td>BTYD</td>
      <td>US</td>
      <td>CA</td>
    </tr>
    <tr>
      <th>2240192</th>
      <td>12503500</td>
      <td>Boulder Bay</td>
      <td>NaN</td>
      <td>32.91401</td>
      <td>-116.93058</td>
      <td>MAR</td>
      <td>US</td>
      <td>CA</td>
    </tr>
    <tr>
      <th>2240193</th>
      <td>12503501</td>
      <td>San Vicente Water Recreation Area</td>
      <td>NaN</td>
      <td>32.92518</td>
      <td>-116.92768</td>
      <td>RGNL</td>
      <td>US</td>
      <td>CA</td>
    </tr>
    <tr>
      <th>2240194</th>
      <td>12503502</td>
      <td>El Capitan Dam</td>
      <td>NaN</td>
      <td>32.88605</td>
      <td>-116.80894</td>
      <td>SPLY</td>
      <td>US</td>
      <td>CA</td>
    </tr>
    <tr>
      <th>2240195</th>
      <td>12503503</td>
      <td>El Capitan Water Recreation Area</td>
      <td>NaN</td>
      <td>32.87421</td>
      <td>-116.79520</td>
      <td>PRK</td>
      <td>US</td>
      <td>CA</td>
    </tr>
  </tbody>
</table>
<p>2240196 rows × 8 columns</p>
</div>




```python
# creation of dictionary of states wherein battles of the civil war ocurred 
civil_war_states = ['AL', 'AR', 'CO', 'FL', 'GA', 'ID', 'IN', 'KS', 'KY', 'LA', 'MD', 'MN', 'MS', 'MO', 'NC', 'ND', 'NM', 'OH', 'OK', 'PA', 'SC', 'TN', 'TX', 'VA', 'VA', 'WV', 'DC']
```


```python
# initialization of empty dataframe
grant_places_df = pd.DataFrame()
```


```python
chunks = []

# process that associates identified locations in text corpus with Geonames gazetteer data
for i in range(len(place_freq_df)):
    place = place_freq_df['Location'][i]
    temp = places.loc[places['name'] == place]
    temp2 = temp.loc[temp['state'].isin(civil_war_states)]
    temp3 = temp2[temp2.featurecode != None]
    chunks.append(temp3)

# dataframe holding Geonames data of identified locations
grant_places_df = pd.concat(chunks, ignore_index = True)
```


```python
# testing efficacy of dataframe
grant_places_df
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>geonameid</th>
      <th>name</th>
      <th>alternatenames</th>
      <th>latitude</th>
      <th>longitude</th>
      <th>featurecode</th>
      <th>countrycode</th>
      <th>state</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>4402300</td>
      <td>Pacific</td>
      <td>Franklin,Pacific,Pasifik,Пасифик</td>
      <td>38.48200</td>
      <td>-90.74152</td>
      <td>PPL</td>
      <td>US</td>
      <td>MO</td>
    </tr>
    <tr>
      <th>1</th>
      <td>5483118</td>
      <td>Pacific</td>
      <td>NaN</td>
      <td>33.53035</td>
      <td>-105.74582</td>
      <td>MN</td>
      <td>US</td>
      <td>NM</td>
    </tr>
    <tr>
      <th>2</th>
      <td>4096937</td>
      <td>West</td>
      <td>NaN</td>
      <td>33.25706</td>
      <td>-85.41662</td>
      <td>PPL</td>
      <td>US</td>
      <td>AL</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4373070</td>
      <td>West</td>
      <td>NaN</td>
      <td>38.22040</td>
      <td>-75.58909</td>
      <td>NaN</td>
      <td>US</td>
      <td>MD</td>
    </tr>
    <tr>
      <th>4</th>
      <td>4450613</td>
      <td>West</td>
      <td>West,Wests Station</td>
      <td>33.19707</td>
      <td>-89.77731</td>
      <td>PPL</td>
      <td>US</td>
      <td>MS</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>2393</th>
      <td>5427779</td>
      <td>Lake</td>
      <td>NaN</td>
      <td>39.24249</td>
      <td>-103.64551</td>
      <td>NaN</td>
      <td>US</td>
      <td>CO</td>
    </tr>
    <tr>
      <th>2394</th>
      <td>5598006</td>
      <td>Lake</td>
      <td>NaN</td>
      <td>44.66687</td>
      <td>-111.39357</td>
      <td>PPL</td>
      <td>US</td>
      <td>ID</td>
    </tr>
    <tr>
      <th>2395</th>
      <td>4407772</td>
      <td>Schofield</td>
      <td>NaN</td>
      <td>37.55226</td>
      <td>-93.20658</td>
      <td>PPL</td>
      <td>US</td>
      <td>MO</td>
    </tr>
    <tr>
      <th>2396</th>
      <td>4595152</td>
      <td>Schofield</td>
      <td>NaN</td>
      <td>33.13266</td>
      <td>-81.19983</td>
      <td>PPL</td>
      <td>US</td>
      <td>SC</td>
    </tr>
    <tr>
      <th>2397</th>
      <td>5438050</td>
      <td>Schofield</td>
      <td>NaN</td>
      <td>39.04110</td>
      <td>-107.05505</td>
      <td>NaN</td>
      <td>US</td>
      <td>CO</td>
    </tr>
  </tbody>
</table>
<p>2398 rows × 8 columns</p>
</div>




```python
# export of dataframe in the form of csv file
grant_places_df.to_csv('grant_places_df.csv')
```

## GIS Visualization Available at: https://arcg.is/5OvLy0

## Written Discussion

Having gained a keen interest in the potential provisioned by data and computational methods pertinent to Geographic Information Systems (GIS), I gravitated towards a project that would geographically map locations relevant to a dataset. For my final project, I chose to ground my application of GIS computational methods on a text corpus, specifically, the Personal Memoirs of Ulysses S. Grant, the 18th President of the United States of America, and the Commanding General of the Union Army throughout the course of the United States’ Civil War. Published in 1885, and accessed digitally via the Project Gutenberg, Grant’s Personal Memoirs detail the President’s extensive military career. Using this text corpus, I sought to identify what those geographical locations most significant to Grant throughout the course of the Civil War were, given their mention in his memoirs?

I chose to hone my engagement with this text on those chapters relevant to the history of the Civil War, spanning from Volume 1’s Chapters 16 through to Chapter 39. The reason why my engagement with this text did not consider the full extent of Grant’s memoirs are due to technical issues with my computational approach to extracted data–which I shall detail in future portions of this Discussion.

The preparation of my project’s data focused on deriving the contents of this text in a format that would be compatible with my chosen analytical computational methods. Utilizing methods of Web Data Mining and Topic Modeling, I was able to extract the digital contents of these memoirs from their original HTML formatting into an organized dataframe in a CSV format. This approach was successful, and this data was in a form wherein the text’s paragraphs were each accessible through methods of accessing a dictionary data structure with the methods provisioned by the Python language, and its associated Pandas, Collections, and Regular Expression Operations libraries.

My analysis of this newly formatted data focused on the application of Named Entity Recognition computational methods provisioned by Python’s spaCy library. I was successfully able to utilize methods of Named Entity Recognition to identify Geopolitical Entities (GPE-tagged) and Physical Locations (LOC-tagged) emplaced across Grant’s text, and subsequently organize them into a secondary dataframe holding the GPE/LOC entity’s name, and its frequency throughout the text. Once in this dataframe organization, I could begin my association of these identified GPE/LOC entities with their corresponding geographical data–that is, their latitude and longitudes. In order to do this, I relied extensively on the GeoNames geographical database. With access to a CSV dataset pertaining to geographical entities in the United States, I was able to relate the entities as mentioned in Grant’s text with their real-life counterparts. At this point, I felt it necessary to narrow down these locations based on their correspondence to those states primarily wherein battles of the Civil War have been recorded to have occurred. Referencing the National Park Service’s information on those states wherein such conflicts occurred, I was able to significantly note only those conflicts that occurred within the boundaries of these states.

Once I had attained the latitudes, longitudes, and other geographical information pertinent to those geographical regions of significance to my project, I was able to process this information in CSV format, and then map it utilizing ArcGIS software. This mapping has produced a visualization that successfully displays the locations referenced by Grant throughout these chapters of primary concern. Emplacing this visualization over David Rumsey’s Civil War Map provides users with the opportunity to view these mapped locations over states labeled in accordance to their position in the Civil War–that is, their belonging to the Union, the Confederacy, or positioning as boundary states amidst this conflict. This project’s replication provisions potential for thorough visualizations of an individuals self-reported, lived experiences. It provisions access into the geographical extent that the Civil War occupied, permitting scholars the opportunity to fathom historical accounts of this, among other, events in a manner that mere text does not.

As mentioned previously, my engagement with the Personal Memoirs of U. S. Grant were limited. This was the result of my approach’s inability to narrow down these identified geographical entities in a manner that would be easily processed by mine and other’s machines in a concise database. As evident in my visualization, there are many mapped locations that are repeated both within, and across, state-lines. With many geographical entities in these states sharing the same name, I was not able to derive a method through which I may only consider those exact geographical entities Grant explicitly refers to in his memoirs. This error emphasizes the need for projects in the Digital Humanities to hold in high priority the direct human reading and engagement with primary sources, like Grant’s memoirs. Although, this project does permit a surface-level engagement with this large and extensive work, and also provides details into the course of the Civil War.

## Citations

GeoNames, https://www.geonames.org/. Accessed 2022.

Grant, Ulysses S. “The Project Gutenberg eBook of Personal Memoirs of U. S. Grant, Complete, by Ulysses S. Grant.” Project Gutenberg, 1 June 2004, https://www.gutenberg.org/cache/epub/4367/pg4367-images.html#ch40. Accessed 2022.

“Places - The Civil War.” United States National Park Service, 23 April 2015, https://www.nps.gov/civilwar/places.htm. Accessed 2022.
Rumsey, David. “Civil War Maps.” David Rumsey Map Collection, http://www.davidrumsey.com/Civil.htm. Accessed 2022.

Saxton, Micah, and Peter Nadel. Named Entity Recognition. CLS-0161 Lesson on Named Entity Recognition Computational Methods in Python. 7 November 2022. Canvas. Accessed 2022.

Saxton, Micah, and Peter Nadel. Topic Modeling. CLS-0161 Lesson on Topic Modeling Computational Methods in Python. 7 November 2022. Canvas. Accessed 2022.

Talmadge, Carolyn C. Geographic Information Systems (GIS). Presentation on Geographic Information Systems (GIS) Computational Methods in Python. 14 November 2022. Canvas. Accessed 2022.

## Thank you, Dr. Micah Saxton and Peter Nadel, for all your help in realizing this project into fruition.