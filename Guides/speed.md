# Размисли върху скоростта на печат ...
### ...и защо няма "значение".

## Няколко термина.
* Скорост на печат (Speed, Velocity, Feedrate) - Каква е *желаната* скорост за движение на главата. Измерва се в мм/с.
* Ускорение (Acceleration) - Колко бързо може да ускорява главата. Измервасе в мм/с².

## Разаснение за скоростта и ускорението.
За да се движи един обект с дадена скорост той трябва да ускори до тази скорост и да може да я поддържа. 
При 3Д притерите това зависи от моторите и съпротивителните сили. Затова и скростта която задаваме е *желана*, 
защото движението може да е по-късо от необходимото ни разстояние или да нямаме сила да развием тази скорост, да не забравяме - 
трябва да спрем или променим посоката на движение след извършване на движението. 
Ако подадем на принтера параметри, който той не може да изпълни то той ще избуксува или в краен случай ще претовари 
мотора и платката която я управлява и може да има перманентни щети. При буксуване (Step Skipping) това което се случва е мотора пропуска стъпки
и се получава изместване (Layer Shifting) по остта по която е станало буксуването, защото той си мисли че е изпълнил правилно командата,
но реално главата се е преместила по-малко. Понеже FDM технологията е бавна, а ние искаме да печатаме бързо търсим начини как да ускорим работата на принтера.
Ако вдигнем скростта ще печатаме по-бързо, но ако нямаме достатъчно дълга права линия започваме да търсим по-добро ускорение.

## Ограниченията.
Максималната скорост при празно движение е доста висока, общо взето зависи от това колко бързо могат да се въртят моторите и колко ни е дълго движението. 
Но ограничението при печат/екструдиране идва от възможността на екструдера да екструдира материал. Разбира се с условието, че няма други фактори, а то има, 
но за тях по-късно, нека се съсредоточим върхучистата максимална скорост. 

- Как да намерим максимална скорост на печат - пускаме екструдера колкото се може по-бързо да екструдира и наблюдаваме кога ще прескочи мотора или ще прегрезе материала, 
ако върти и не излиза материал значи е прегрезан, а ако се чуват странни чукащи звуци от мотора то той превърта, като знаем колко е дебел филамента
то можем да намерим колко е обема материал който може екструдера ни да избутва (измерва се в мм³/с). 
След като знаем, дебелината и височината на линията която искаме да напечатаме, намираме и нашата максимална скорост с която може да ни се движи главата. 
Ако избутваме 35мм/с, то обработваме 8.42 mm³/s, при височина 0.2мм и широчина 0.44мм е 90мм/с. (Ако намалим височината то и скоростта ще се увеличи)

Но не винаги имаме място да развием такива скорости и за да постигнем по-бърз резултат трябва да вдигнем ускорението.
- Как да намерим ускорението - ограничението там е силата на моторите и най-тежката част. При принтерите където леглото се движи в печатащата равнина,
почти винаги то е ограничителя. Та пускаме движение по Y с различни ускорения и гледаме кога мотора ще започне да прескача, 
това ускорението което не бива да надвишаваме. Ако ускорението ни е 2000мм/с² то ще достигаме нашата желана скорост след 2мм, да не забравяме че трябва и да спрем това ще ни отнеме още 2мм,
ако имаме линия 10мм то 40% от времето ще ускоряваме и ще спираме, при ускорение 5000мм/с², то вече сме на 0.8мм, не звучи много, 
но когато имаме обекти с честа смяна на посоката това си оказва влияние.

## Когато завиваме не винаги ни се налага да спираме или какво е Jerk, Squere corner velocity и подобни.
Това е възможността на устройството да променя посоката си при завой в прав ъгъл, като това служи за основа за възможностите му да направи по-остър завой или по-плавен.
Отново основното ограничение е мотора. Ако можем да завием с по-висока скорост, няма да има нужда да държим спирачката и в последствие да ускоряваме по-дълго след ъгъла. 

## Да добавим още няколко ограничения.
До сега гледахме на нещата от гледна точка на скорост, намерихме максималното което можем да направим, но качеството ако въобще печата го няма никакво.
- При резки промени в скоростта машината трепти и се получава "ехо"(Ghosting, Ringing) на ъгъла по продължение на права едно е да ускоряваш, 
често решенията са намаляване на скоростта и ускорението, с цел да се ограничат вибрациите. (Друга причина са разхлабени или некачествени ремъци и движения.)
Съществува и софтуеърно решение (InputShaper), което се стреми да не позволява принтера да влиза в хармонични трептения.
- Налягане в дюзата - за да излиза материал трябва да има налягане в дюзата, при рязко ускорение и висока скорост принтерът прави по-тънка нишка (UnderExtrusion),
до достигане на оптимално налягане, а когато започне да спира се получава точно обратният ефект освобождава налягането при спиране и прави по-дебела нишка(OverExtrusion).
Решението е или се намалява скоростта и ускорението или се използва алгоритъм който да увеличи налягането на екструдера при ускорение и да намали налягането при спиране. (Pressure Advance, Linear Advance и пр.)
Като тази стойност която се задава на алгоритъма варира спрямо материал, температура, дюза и тн. Примерно ако имаме 0.64 коефициент, то групо при ускорение ще увеличава екструдираният материал със 64%.
Ако целтяната ни скорост е 90мм/с и екструдера избутва максимално по 35мм/с, то при ускорение ще се помъчи да избута 57.4мм/с, което е над възможностите ни.

