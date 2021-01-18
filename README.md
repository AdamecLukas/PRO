#include <iostream>
#include <string>
#include <fstream>

using namespace std;

 // ---  musí byť uložený v súbore s názvom „phone_book.txt“. ---
const char* phonebook_filename = "phone_book.txt";

 //  skupina  //  Výpočet skupín kontaktov pre ľahší prístup
enum groups {Rodina, priatelia, kolegovia, VIP, ini};

 // --- Každý záznam v telefónnom zozname obsahuje  informácie 
struct contact_info
{
	// pre telefónne čísla sa uprednostňuje reťazec, ktorý umožňuje znamienko, koncovku a znak -
	string phone_number;
	string contact_name;
	groups contact_group;
	 
	contact_info* next_element = NULL;
};


// --- Telefónny zoznam sa má udržiavať ako prepojený zoznam. Prepojený zoznam je reťazec položiek, ktoré majú ---
// --- vzájomné odkazy. 

contact_info* phonebook = NULL;

 // --- Musí existovať premenná, ktorá obsahuje počet záznamov v telefónnom zozname s názvom N_entries. ---
unsigned int N_entries = 0;

 // --- Musí existovať jednorozmerné 26-prvkové pole s názvom look_up, ktorého prvky sú --- // --- ukazovatele na štruktúru contact_info.
contact_info* look_up[26] = { NULL };

void upcase(string&);			// Toto je prototyp funkcie, ktorý by konvertoval reťazec na veľké písmená
contact_info* create_entry();		//  prototyp funkcie, ktorý získava vstup od používateľa a ukladá ho do nového kontaktného objektu.
void contact_insert(contact_info*);	// prototyp funkcie, ktorý vloží nový kontaktný objekt do telefónneho zoznamu
 // Nasledujúca funkcia sa používa pre obidva body C.
contact_info* contact_find(string);	//  na vyhľadanie a nájdenie objektu kontaktu na základe názvu
void contact_show(contact_info);	//  na zobrazenie informácií o kontaktnom objekte
void contact_find_show_delete();	//  na vyhľadanie a odstránenie kontaktov (bod C projektu)
void contact_find_show();		//  na vyhľadanie a zobrazenie mena v telefónnom zozname (plus niekoľko chýb)
void phonebook_save();			//  na ukladanie údajov do súboru (bod E
void phonebook_load();			//  na načítanie dátového súboru (voliteľný, nebol v projekte požiadaný)
int main()
{
	// Čítanie súboru telefónneho zoznamu 
	phonebook_load();
	 // --- Po spustení programu poskytuje  menu: ---
	char userchoice;
	do
	{
		cout << endl;
		cout << "a. Vytvorte novy zaznam." << endl;
		cout << "b. Odstranit zaznam." << endl;
		cout << "c. Najdite zaznam a zobrazte jeho informacie. " << endl;
		cout << "d. Cely telefonny zoznam ulozte do suboru. " << endl;
		cout << "e. koniec. " << endl;
		cout << endl << "Zadajte svoj vyber: ";
		cin >> userchoice;
		switch (userchoice)
		{
			 // --- Ak si zvolí (a), musí byť napisat info
		case 'a': case 'A':
			contact_insert(create_entry());
			break;
			 // --- Ak si  zvolí písmeno b), musí byť požiadaný o meno osoby
		case 'b': case 'B':
			contact_find_show_delete();
			break;
			 // --- Ak si užívateľ zvolí (c), musí byť požiadaný o meno osoby .
		case 'c': case 'C':
			contact_find_show();
			break;

			// --- musí byť uložený celý telefónny zoznam 
		case 'd': case 'D': case 'e': case 'E':
			phonebook_save();
			break;
			
		default:
			cout << endl << "!!! Neplatna volba !!!" << endl;
		}
		 // --- Ak si užívateľ zvolí (e) program bude ukončený 
	} while ((userchoice != 'e') && (userchoice != 'E'));
	cout << "Program sa ukoncuje..." << endl;
	return 0;
}

//  Všetko, aj keď je zadané malými písmenami, musí byť uložené veľkými písmenami.
void upcase(string& str)
{
	for (int i = 0; i < str.length(); i++) str[i] = toupper(str[i]);
}

 // --- Ak si užívateľ zvolí (a), musí byť požiadaný o zadanieinformácií ( číslo, meno kontaktu a skupina).
contact_info* create_entry()
{
	// Vytvorenie nového prázdneho kontaktu
	contact_info* new_entry = new contact_info;
	cout << "Zadajte meno: ";
	cin.ignore();	// Vymazávame vstupný buffer, aby sme mohli používať funkciu getline
	getline(cin, new_entry->contact_name);
	upcase(new_entry->contact_name);
	cout << "Zadajte telefonne cislo: ";
	getline(cin, new_entry->phone_number);
	upcase(new_entry->phone_number);
	cout << "Zadajte skupinu" << endl;
	cout << "1, f or F = rodina" << endl;
	cout << "2, r or R = priatelia" << endl;
	cout << "3, c or C = Kolegovia" << endl;
	cout << "4, c or c = VIP" << endl;
	cout << "alebo zadajte iny znak pre skupinu (Ostatne)" << endl;
	cout << "Tvoja volba: ";
	char groupchoice;
	cin >> groupchoice;
	switch (groupchoice)
	{
	case '1': case 'f': case 'F':
		new_entry->contact_group = Rodina;
		break;
	case '2': case 'r': case 'R':
		new_entry->contact_group = priatelia;
		break;
	case '3': case 'c': case 'C':
		new_entry->contact_group = kolegovia;
		break;
	case '4': case 'v': case 'V':
		new_entry->contact_group = VIP;
		break;
	default:
		new_entry->contact_group = ini;
	}
	new_entry->next_element = NULL;
	cout << "Novy kontakt bol uspesne vytvoreny." << endl;
	return new_entry;
}



// --- Zakaždým, keď je zavedený nový záznam, musí sa vyhľadať pole look_up, aby sa zistilo, kde sa nachádza
// --- prepojený zoznam, je vhodným miestom na vloženie záznamu.
void contact_insert(contact_info* newcontact)
{
	// Ak je nová položka 1. položka, máme iní prípad
	if (N_entries == 0 || phonebook->contact_name.compare(newcontact->contact_name) > 0)
	{
		newcontact->next_element = phonebook;
		phonebook = newcontact;
	}
	else
	{
		contact_info* previous = phonebook;
		contact_info* next = phonebook->next_element;
		while (next != NULL)
		{
			if (newcontact->contact_name.compare(next->contact_name) < 0) break;
			previous = next;
			next = next->next_element;
		}
		previous->next_element = newcontact;
		newcontact->next_element = next;
	}
	N_entries++;
}


// Vyhľadá a vráti ukazovateľ na kontakt na základe mena kontaktu alebo NULL, ak sa nenájde.
contact_info* contact_find(string Name)
{
	contact_info* contact = phonebook;
	while (contact != NULL)
	{
		if (Name.compare(contact->contact_name) == 0)
			return contact;
		else
			contact = contact->next_element;
	}
	return NULL;
}

// Vytlačí kontaktné informácie.
void contact_show(contact_info* contact)
{
	cout << "Contact Name: " << contact->contact_name << endl;
	cout << "Contact Number: " << contact->phone_number << endl;
	cout << "Contact Group: ";
	switch (contact->contact_group)
	{
	case Rodina: cout << "Rodina" << endl; break;
	case priatelia: cout << "priatelia" << endl; break;
	case kolegovia: cout << "kolegovia" << endl; break;
	case VIP: cout << "VIP" << endl; break;
	default: cout << "ini" << endl;
	}
}
//  Nájdenie, zobrazenie, potvrdenie a odstránenie kontaktu
void contact_find_show_delete()
{
	cout << "Zadajte meno kontaktu, ktory sa ma vymazat : ";
	string Name;
	cin.ignore();
	getline(cin, Name);
	upcase(Name);
	cout << endl;
	contact_info* contact = contact_find(Name);
	if (contact != NULL)
	{
		cout << "Nasledujuci kontakt sa vymaze:" << endl;
		contact_show(contact);
		cout << "Zadajte Y/y potvrdit odstranenie:";
		char confirm;
		cin >> confirm;
		if (confirm == 'Y' || confirm == 'y')
		{
			// Ak má byť vymazaná 1. položka, máme iní,specialní prípad
			if (contact == phonebook)
			{
				phonebook = phonebook->next_element;
			}

			// V opačnom prípade by sme mali nájsť predchádzajúci kontakt na prepojenie
			else
			{
				contact_info* previous = phonebook;
				while (previous->next_element != contact) previous = previous->next_element;
				previous->next_element = contact->next_element;
			}
			// Uvoľnená pamäť pre vymazaný kontakt
			delete contact;
			N_entries--;
			cout << "kontakt uspesne vymazany" << endl;
		}
		else cout << "Kontakt sa neodstranil." << endl;
	}
	else cout << "Nenasiel " << Name << " v phonebook!!!" << endl;
}
void contact_find_show()
{
	cout << "Zadajte meno kontaktu, ktory sa ma vyhladat ";
	string Name;
	cin.ignore();
	getline(cin, Name);
	cout << endl;
	upcase(Name);
	contact_info* contact = contact_find(Name);
	if (contact != NULL)
		contact_show(contact);
	else
		cout << "Nenasiel " << Name << " v phonebook!!!" << endl;
}
// --- celý telefónny zoznam musí byť uložený v súbore
void phonebook_save()
{
	ofstream phonebook_file;
	// --- volá sa „phone_book.txt“.
	phonebook_file.open(phonebook_filename);
	cout << "Pisanie " << N_entries << " zaznamy do " << phonebook_filename << " ..." << endl;
	// --- Na prvom riadku musí byť uložený počet záznamov
	phonebook_file << N_entries << endl;
	contact_info* current_item = phonebook;
	while (current_item != NULL)
	{
		// --- Záznamy musia byť oddelené od seba navzájom a od prvého riadku riadkom dvadsiatich 
		phonebook_file << "********************" << endl;
		// --- Pre každú položku musí byť meno uložené v prvom riadku
		phonebook_file << current_item->contact_name << endl;
		phonebook_file << current_item->phone_number << endl;
		switch (current_item->contact_group)
		{
		case Rodina: phonebook_file << "rodina" << endl; break;
		case priatelia: phonebook_file << "priatelia" << endl; break;
		case kolegovia: phonebook_file << "kolegovia" << endl; break;
		case VIP: phonebook_file << "VIP" << endl; break;
		default: phonebook_file << "ini" << endl;
		}
		current_item = current_item->next_element;
	}
	phonebook_file.close();
}
// Táto funkcia načíta predvolený súbor kontaktov 
void phonebook_load()
{
	ifstream phonebook_file;
	phonebook_file.open(phonebook_filename);
	if (!phonebook_file.is_open())
	{
		cout << "Phonebook subor sa nepodarilo otvorit !!!" << endl;
	}
	else
	{
		phonebook_file >> N_entries;
		contact_info** previous = &phonebook;
		string text;
		// Tu preskočíte návrat na koniec riadku 
		getline(phonebook_file, text);
		cout << "citanie " << N_entries << " záznamov kontaktov ..." << endl;
		for (int i = 0; i < N_entries; i++)
		{
			contact_info* new_entry = new contact_info;
// Preskočenie riadku 
			getline(phonebook_file, text);
			getline(phonebook_file, new_entry->contact_name);
			getline(phonebook_file, new_entry->phone_number);
			getline(phonebook_file, text);
			if (text.compare("rodina") == 0)  	new_entry->contact_group = Rodina;
			else if (text.compare("priatelia") == 0)	new_entry->contact_group = priatelia;
			else if (text.compare("kolegovia") == 0)	new_entry->contact_group = kolegovia;
			else if (text.compare("VIP") == 0)	new_entry->contact_group = VIP;
			else new_entry->contact_group = ini;
			new_entry->next_element = NULL;
			*previous = new_entry;
			previous = &new_entry->next_element;
		}
		phonebook_file.close();
	}
}
