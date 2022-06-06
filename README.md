package com.company;
import java.util.Scanner;
import java.time.LocalTime;
import java.io.File;
import java.io.IOException;
public class Main {

    public static void main(String[] args) throws IOException {
        Scanner input = new Scanner(System.in);
        String username;
        String password;


        System.out.println("Zadajte username: ");
        username = input.nextLine();

        System.out.println("Zadajte password: ");
        password = input.nextLine();

        Pocitac check = new Pocitac(username, password);

        if (check.auth()) {
            System.out.println("Ste prihlásený!");
            LocalTime myObj = LocalTime.now();
            System.out.println("Aktuálny čas je: " + myObj);
        } else {
            System.out.println("Zadali ste zlé meno alebo heslo!!");
            System.exit(0);
        }
        Scanner sc = new Scanner(System.in);
        System.out.println("Zadajte príkaz, ktorý chcete použiť!: ");


        String help = input.next();
        if (help.equals("help")) {
            System.out.println("help= vypíše všetky príkazy " + "countInputLetters= vypíše kolko písmen obsahuje napísané slovo " +
                    ";makefile=pridá do files názov súboru " + ";dir= vypíše všetky súbory "  + ";logout=odhlásenie ");
        }

        String dir = input.next();
        if (dir.equals("dir")) {
            File directoryPath = new File("C:\\xampp");
            String contents[] = directoryPath.list();
            System.out.println("Zoznam súborov:");
        }
        String makefile = sc.next();
        if (makefile.equals("makefile")) {
            try {
                System.out.print("Enter a filename: ");
                String file = input.next();
                File f = new File(file);
                if (f.createNewFile()) {
                    System.out.println(f.getName() + " created.");
                } else {
                    System.out.println("File already exists.");
                }
            } catch (IOException e) {
                System.out.println("An error occurred.");
                e.printStackTrace();
            }

        }


        String countinputletters = sc.next();
        if (countinputletters.equals("countinputletters")) {
            System.out.print("Type a word: ");
            String word = input.next();
            int stringLength = word.length();
            int vowels = 0;
            char temp;
            for (int i = 0; i < word.length(); i++) {
                char ch = word.charAt(i);
                if (ch == 'a' || ch == 'e' || ch == 'i' || ch == 'o' || ch == 'u' || ch == 'y') {
                    vowels++;
                }
            }
            System.out.println(word + " obsahuje " + stringLength + " písmen & " + vowels + " samohlások.");
        }

    }
}
