package com.company;
import java.util.Scanner;
public class Main {

    public static void main(String[] args)
    {
        int a, b, i, prvy, druhy, treti, f, x;
        Scanner scan = new Scanner(System.in);

        System.out.print("Zadaj velkost pola: ");
        a= scan.nextInt();

        int[] arr = new int[a];

        System.out.print("Zadaj " +a+" cisla: ");
        for(i=0; i<a; i++)
            arr[i] = scan.nextInt();

        for(i=0; i<(a-1); i++)
        {
            for(f=0; f<(a-i-1); f++)
            {
                if(arr[f]>arr[f+1])
                {
                    x = arr[f];
                    arr[f] = arr[f+1];
                    arr[f+1] = x;
                }
            }
        }
        System.out.println("\nZoradene pole:");
        for(i=0; i<a; i++)
            System.out.print(arr[i]+ " ");


        System.out.print("\n\nZadaj cislo, ktore chces vyhladat: ");
        b = scan.nextInt();

        prvy = 0;
        druhy = a-1;
        treti = (prvy+druhy)/2;

        while(prvy<=druhy)
        {
            if(arr[treti]<b)
                prvy = treti+1;
            else if(arr[treti]==b)
            {
                System.out.println("\nCislo bolo najdene v kroku:" +treti+ ", Pozicia cisla:" +(treti+1));
                break;
            }
            else
                druhy = treti-1;

            treti = (prvy+druhy)/2;
        }

        if(prvy>druhy)
            System.out.println("\nCislo, nieje v poli :(");
    }
}
