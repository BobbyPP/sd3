

	~~ Detalii legate de implementarea temei de casa 3 ~~

	~~ Descrierea comportamentului functiilor aditionale

>
int** make_copy_of_Ma ( TGraphM, int*** )

	Functia primeste structura grafului, si construieste o copie a matricei aferente grafului, pe care o returneaza.

>
void make_zeros_on_lines ( TGraphM* )

	Functia primeste prin referinta graful cu structura sa si, pentru fiecare linie din matricea de adiacenta aferenta, realizeaza urmatorul set de instructiuni: mai intai parcurge linia (care este un vector) pentru a afla elementul minim, apoi din fiecare element de pe linie scade valoarea acelui element minim.

>
void make_zeros_on_columns ( TGraphM* )

	Asemanator cu functia precedenta, functia primeste prin referinta structura grafului si, pentru fiecare coloana din matricea de adiacenta, scade din fiecare element de pe coloana respectiva valoarea elementului minim obtinuta pe acea coloana. Functia nu returneaza nimic explicit, dar modifica matricea de adiacenta trimisa ca parametru prin referinta.

>
int exactly_one_zero ( int*, int*, int )

	Functia primeste doi vectori si lungimile lor (care sunt egale). Primul vector este ori o linie, ori o coloana din matricea de adiacenta a grafului, iar al doilea are numai valori de -1, 0 si 1. Functia verifica daca exista exact o pozitie cu proprietatea ca pe aceasta ambii vectori sa aiba valoarea zero. Valorea de return este pozitia respectiva, sau -1 in caz ca nu exista asemenea pozitie sau exista mai multe asemenea pozitii.

>
void make_assignments_once ( TGraphM*, int**, int**, int***, char** )

	Functia primeste mai multi parametri: graful, cu structura sa (numar de noduri, matrice de adiacenta), doi vectori ce retin pozitiile unor elemente zero din matricea de adiacenta a grafului, o matrice care marcheaza elementele zero din matricea de adiacenta si un vector care retine in fiecare element daca linia cu acelasi index, corespunzatoare matricei de adiacenta, este 'marcata' sau nu.
	Matricea de adiacenta primita trebuie sa aiba cel putin un zero pe fiecare linie si cel putin un zero pe fiecare coloana. Aceasta functie lucreaza in principiu cu matricea care marcheaza elementele zero.
	Astfel, pentru fiecare linie, se verifica daca exista exact un zero pe pozitia caruia, in matricea de marcare, elementul sa fie egal tot cu zero (Elementele marcate cu zero in acea matrice sunt, practic, elementele nevizitate. Insa elementele de interes sunt doar cele cu valoarea zero in matricea de adiacenta.) Daca se indeplineste conditia, se ia coloana de care apartine acel zero (pe linia x) in matricea de adiacenta si, pentru fiecare pozitie de pe acea coloana in care elementul este zero (cu exceptia zero-ului de pe linia x), in matricea de marcare se pune valoarea -1 fix la aceeasi pozitie. Cu alte cuvinte, se marcheaza zero-urile de pe coloana respectiva cu valoarea -1. Exceptie face zero-ul de la care am plecat (cel de pe linia x), care se marcheaza cu valoarea 1. Se itereaza apoi mai departe, fie ca se indeplineste sau nu conditia.
	Urmatorul pas al functiei este similar cu primul, decat ca zero-urile nemarcate (avand 0 in matricea de marcare, la aceeasi linie si coloana) se cauta pe coloane. Conditia este sa se gaseasca exact un zero pe coloana respectiva (y) si sa fie nevizitat (nemarcat). Atunci, se iau toate zero-urile de pe linia de care apartine elementul zero (cel de pe coloana y, de la care plec) si se marcheaza cu valoarea -1 in matricea de marcare. Exceptie face, din nou, elementul zero de la care am plecat, care se marcheaza cu valoarea 1.
	Unul din vectori (l) retine pentru fiecare index x coloana, din matricea de adiacenta, de care apartine fiecare element zero marcat cu 1, aflat pe linia x. Exista si un vector (c) care pentru fiecare index y retine linia, din matricea de adiacenta, de care apartine fiecare element zero marcat cu 1, aflat pe coloana y.
	Exista inca un vector de tip char care selecteaza doar liniile care NU au zero-uri marcate cu 1. In acesti indecsi, vectorul are valoarea 'm', iar in ceilalti (unde liniile corespunzatoare indecsilor au zero-uri marcate cu 1), vectorul are valoarea '\0'.
	De fiecare data cand se analizeaza existenta elementului zero nemarcat pe linie, ori coloana, linia sau coloana respectiva se incarca intr-un alt vector (vMa), si de asemenea si linia sau coloana cu acelasi index corespunzatoare matricei de marcare se incarca intr-un vector (vMatrix).
	Matricea de marcare si vectorii l si c se modifica pe parcurs, si deoarece sunt primiti prin referinta, valoarea lor ramane modificata si in functia din care am facut apelul. Functia nu returneaza nimic.

>
int equal_arrays ( int*, int*, int )

	Functia primeste doi vectori de lungime specificata, egala. Verifica daca vectorii sunt egali (pe fiecare pozitie, elementele dintr-un vector sunt egale cu elementele din celalalt). In caz afirmativ, se returneaza 1. In caz negativ, se returneaza 0.

>
void check_and_thick ( int**, int*, int*, char**, char**, int, int )

	Aceasta functie primeste matricea de marcare (prin referinta), vectorii l si c (care retin pozitiile elementelor zero marcate cu 1), vectorul care retine liniile care nu au zero-uri marcate cu 1 (vectorul 'marked'), o pozitie (pos) care indica pozitia pe o anumita linie pe care se vor face iteratiile la apel, si un vector (thick) care retine coloane marcate.
	Linia pos din matricea de marcare este pe cale de a fi marcata, prin urmare primeste semnul 'c' (de la 'checked', insemnand 'parcursa') in vectorul de marcare a liniilor (marked). Se parcurge linia pos de la un capat la altul, si unde se intalnesc zero-uri marcate cu -1, li se retine coloana pe care s-au gasit si se procedeaza astfel:
		La inceput, nicio coloana nu este marcata in vectorul (de tip char) 'thick', acesta avand toate elementele initializate cu '\0'. Se marcheaza cu 't' in vector coloana respectiva (y), numai daca aceasta nu a fost marcata deja (adica parcursa in aceasta functie). Se poate deduce ca pe coloana y exista exact un zero marcat in matricea de marcare cu 1. Pozitia acestuia este deja retinuta de vectorul 'c' ce retine linia pe care se afla zero-ul marcat cu 1 de pe fiecare coloana corespunzatoare indexului vectorului. Astfel, linia pe care se gaseste zero-ul marcat cu 1 de pe coloana y se marcheaza cu 'm' (ceea ce inseamna 'marked', adica nu este inca 'parcursa') in vectorul de marcare 'marked'.
	Acelasi procedeu il urmeaza toate elementele zero marcate cu -1 de pe linia pos.

>
int smallest_element_not_zero ( TGraphM )

	Functia primeste structura grafului si initializeaza un element caruia ii atribuie ca valoare suma tuturor valorilor din matricea de adiacenta (masura de siguranta pentru ca elementul sa fie mai mare decat 0). Apoi, comparandu-l cu fiecare element din matrice, daca este mai mare decat elementul din matrice iar elementul din matrice este diferit de 0, atunci primeste elementul din matrice. Prin urmare, functia gaseste minimul mai mare decat zero din matricea de adiacenta a grafului si il returneaza.

>
void prepare_for_next_assignment ( TGraphM*, int***, int**, char**, char** )

	Functia primeste graful problemei, cu structura sa, matricea de marcare, vectorul (l) care retine pentru fiecare linie coloanele elementelor zero marcate cu 1, vectorul de marcare a liniilor (marked) si vectorul de marcare a coloanelor (thick). Se initializeaza variabila 'theta' care retine elementul minim din matricea de adiacenta, prin functia smallest_element_not_zero.
	Elementele din matricea de adiacenta care nu se afla pe linii marcate cu '\0' in vectorul 'marked' si nici pe coloana marcate cu 't' in vectorul 'thick' se diminueaza cu valoarea lui theta. Elementele din matrice care se afla pe linii marcate cu '\0', dar nu se afla pe coloane marcate cu 't' (adica se afla pe coloane marcate tot cu '\0') si elementele care se afla pe linii marcate cu 'c' in vectorul 'mark', si se afla pe coloane marcate cu 't' raman neschimbate ca valoare. Restul elementelor din matrice, cele aflate pe linii nemarcate cu 'c' (care sunt marcate in vectorul 'marked' cu '\0'), aflate pe coloane marcate cu 't' (in vectorul de marcare a coloanelor ('thick')) sunt adunate cu valoarea lui theta.
	Tot in aceasta functie se sterg marcarile din matricea de marcare, toate elementele acesteia fiind initializate la valoarea 0. De asemenea, se sterg marcarile si din vectorul de marcare a liniilor ('marked'), din vectorul de marcare a coloanelor ('thick') si din vectorul ce retine coloanele elementelor marcate cu 1 de pe liniile cu indexul corespunzator (l).

>
int all_assignments_done ( int*, int )

	Functia primeste un vector (c) ce retine pozitiile (doar liniile) zero-urilor marcate cu 1 din matricea de adiacenta a grafului, aflate in coloanele specifice indecsilor. Se doreste a se testa daca fiecare coloana are (exact) un zero marcat cu 1, de unde reiese si ca fiecare linie din matricea de adiacenta are exact un zero marcat cu 1. Deoarece vectorul c are valoarea -1 pentru coloanele care nu au niciun zero marcat cu 1, se verifica daca vectorul c are sau nu vreun element egal cu -1. Daca nu are, testul este adevarat, deci functia returneaza valoarea 1. In caz contrar, la test se obtine fals, deci functia returneaza 0.

	~~ Descrierea comportamentului functiei 'mari' si a algoritmului problemei

	Functia 'solve' primeste datele din fisierul primit ca parametru. Pe prima linie este N, numarul de linii si coloane din matricea de adiacenta aferenta grafului (numarul de iepurasi, care este egal cu numarul de vizuine). Dintre lucrul cu liste si cel cu matrice de adiacenta, am ales lucrul cu matrice de adiacenta. Ca metoda de rezolvare, am ales Hungarian Algorithm, problema noastra fiind practic una de asignare (corespondenta) dupa un anumit criteriu (suma distantelor sa fie minima).
	Se citeste numarul de linii si coloane, apoi se parcurge restul fisierului, retinand distanta dintre fiecare iepure si fiecare vizuina. Practic, fisierul reprezinta o matrice in care indecsii liniilor sunt reprezentati de iepuri, iar indecsii coloanelor sunt reprezentati de vizuine.
	Informatia se retine in matricea de adiacenta a grafului de tip TGraphM pe care l-am declarat, iar aceasta se copiaza intr-o alta matrice care va retine informatia initiala (necesara pentru calcularea drumului minim) pentru ca aceasta va fi modificata ulterior. Ambele matrice se aloca dinamic.
	Se initializeaza vectorii l si c si se aloca dinamic. Vectorul l retine coloana elementului marcat de pe linia corespunzatoare indexului, iar vectorul c retine coloana elementului marcat de pe coloana corespunzatoare indexului. Initial, vectorii au toate elementele egale cu -1, pentru a nu fi compatibile cu indexul coloanei, respectiv liniei elementului. Cu alte cuvinte, in dreptul unui element din l sau c, cu valoarea -1, nu se afla niciun element marcat. Se initializeaza si matricea de marcare Matrix, care este de asemenea alocata dinamic, cu numarul de linii si coloane egal cu al matricei de adiacenta. Initial, toate elementele ei sunt zero, practic niciun element nu este marcat initial in matricea de adiacenta.
	O prima modificare a matricei de adiacenta se efectueaza la apelul functiilor make_zeros_on_lines si make_zeros_on_columns, care au comportamentul cel descris mai sus: de pe fiecare linie a matricei de adiacenta se scade elementul minim al liniei respective, apoi la fel se procedeaza si pentru coloane. Se obtine o matrice care are cel putin un zero pe fiecare linie si pe fiecare coloana.
	Se initializeaza vectorii (de tip char) marked si thick cu 'm', respectiv '\0', alocati dinamic.
	Se incepe etapa de asignare pe matricea de adiacenta. Se incepe cu asignarea pe linii. Se porneste de pe linia de sus. Daca exista exact un zero pe linie, se marcheaza acea pozitie (linie, coloana) cu 1 (vizitat) in matricea de marcare Matrix, iar toate elementele zero din matricea de adiacenta de pe coloana pe care s-a gasit elementul zero, se 'taie', adica se initializeaza cu -1 in matricea Matrix. Se trece la linia urmatoare si se aplica acelasi algoritm, insa doar pentru zero-urile din matricea de adiacenta ale caror pozitii sunt marcate cu 0 in matricea Matrix. Dupa ce se parcurg astfel liniile, tot asa se parcurg si coloanele: pe fiecare coloana din matricea de adiacenta se cauta daca exista exact un zero netaiat si nevizitat. Daca se gaseste, se ia linia pe care se gaseste si, se 'taie' fiecare zero care nu era vizitat sau taiat. Deoarece si dupa aceasta etapa s-ar putea ca nu toate zero-urile din matricea de adiacenta sa fie vizitate sau taiate (marcate cu 1 sau -1 in Matrix) se reia intreg algoritmul incepand din nou cu liniile. Se poate relua de mai multe ori, insa de cel mult n ori (unde n este numarul de linii/coloane ale matricei de adiacenta). Iterarea pana la n nu este eficienta, de aceea se seteaza conditia de oprire ca fiind atunci cand vectorul l ramane neschimbat (adica atunci cand situatia elementelor marcate si a celor taiate ramane aceeasi pentru o iteratie intreaga). Se putea verifica aceasta conditie in mod echivalent si pentru vectorul c. Dupa oprire, se verifica daca liniile/coloanele au exact un zero asignat (marcat cu 1 in Matrix).
	->	In caz negativ, se aplica algoritmul descris de functiile check_and_thick si prepare_for_next_assignment.
	Functia check_and_thick se face pentru liniile marcate cu 'm' (avand 'm' pe acelasi index in vectorul marked). Initial, toate liniile sunt marcate. Se porneste pe fiecare linie marcata cu 'm' (liniile marcate cu 'm' nu contin elemente zero marcate), si se parcurge linia. Pentru fiecare element zero 'taiat', se retine si se parcurge coloana acestuia, iar acolo unde se intalneste elementul zero marcat (exista exact un element zero marcat pe coloana respectiva), se pune marcheaza cu 'c' linia pe care se gaseste acest zero marcat. De asemenea, coloana parcursa se marcheaza in vectorul thick cu 't', la indexul corespunzator coloanei. Se parcurg restul elementelor de zero taiate (daca mai exista) de pe linia marcata cu 'm'. Pentru fiecare zero taiat se repeta algoritmul tocmai prezentat. Dupa parcurgerea liniei, aceasta se marcheaza cu 'c' in vectorul marked, la indexul corespunzator.
	Functia prepare_for_next_assignment este apelata in continuarea algoritmului descris in paragraful trecut. Aceasta calculeaza, mai intai, elementul minim mai mare decat zero al matricei de adiacenta, apoi: il scade din toate elementele de pe pozitiile (linie marcata cu 'c'; coloana nemarcata cu 't') si il aduna la toate elementele de pe pozitiile (linie nemarcata cu 'c'; coloana marcata cu 't'). Restul elementelor din matricea de adiacenta raman cu valoarea neschimbata. Tot in aceasta functie, informatiile acumulate in vectorii marked, thick, l si in matricea de marcare Matrix sunt sterse, iar acesti vectori si matrice se initializeaza din nou (marked cu 'm', thick cu '\0', l cu -1 si Matrix cu 0) pentru toate elementele.
	Se reia algoritmul incepand cu paragraful 77.
	->	In caz afirmativ, se ia in considerare matricea de adiacenta nemodificata (cea corespunzatoare grafului de la inceput), adica cea retinuta in matricea initial_Ma. Avand pozitiile tuturor elementelor asignarii retinute in vectorii l si c, se iau elementele de pe acele pozitii din matricea initial_Ma si se aduna. Suma se retine in variabila sum_of_the_distances, a carei valoare este returnata de functie si reprezinta rezultatul problemei.
