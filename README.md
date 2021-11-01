import java.util.Scanner;

class Main {
  public static void main(String[] args) {

    char operator;
    Double  cislo1,cislo2, vysledok;

    
    Scanner input = new Scanner(System.in);

    System.out.println("Vyber funkciu: +, -, *, /");
    operator = input.next().charAt(0);

    
    System.out.println("Zadaj 1 cislo:");
    cislo1= input.nextDouble();

    System.out.println("Zadaj 2 cislo:");
    cislo2= input.nextDouble();

    switch (operator) {

      
      case '+':
        vysledok= cislo1+ cislo2;
        System.out.println(cislo1+ " + " + cislo2+ " = " + vysledok);
        break;

      
      case '-':
        vysledok= cislo1- cislo2;
        System.out.println(cislo1+ " - " + cislo2+ " = " + vysledok);
        break;

      
      case '*':
        vysledok= cislo1* cislo2;
        System.out.println(cislo1+ " * " + cislo2+ " = " + vysledok);
        break;

      
      case '/':
        vysledok= cislo1/cislo2;
        System.out.println(cislo1+ " / " + cislo2+ " = " + vysledok);
        break;

      default:
        System.out.println("Zly operator!");
        break;
    }

    input.close();
  }
}
