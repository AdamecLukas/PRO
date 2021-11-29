package com.company;
import java.util.Arrays;

public class Main {

    public static void main(String[] args) {
        System.out.println("Random čísla:");
        int[] RANDOM= new int[20];
        int q=0;
        int w=0;
        int e=0;
for (int i=0; i<20; i++){
    int random_cislo=(int)(Math.random()*50);
    RANDOM[i]=random_cislo +1;
    q=RANDOM[i];
    e=q+w;
    w=e;
}
for (int pole : RANDOM){
    System.out.println("" + pole);
}

float priemer=e/20;

System.out.println("Súčet čísel : "+e);
        Arrays.sort(RANDOM);
        System.out.println("________");
System.out.println("Najväčšie číslo: "+ RANDOM[19]);
        System.out.println("________");
System.out.println("Najmenšie číslo: " + RANDOM[0]);
        System.out.println("________");
        System.out.println("KONIEC !");

    }
}
