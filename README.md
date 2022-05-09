//Bojovnik
package com.company;


public class Bojovnik {
    private String meno;
    private int zivot;
    private int maxZivot;
    private int utok;
    private int obrana;
    private Kocka kocka;
    private String sprava;

    public Bojovnik(String meno, int zivot, int utok,int obrana, Kocka kocka){
        this.meno=meno;
        this.zivot = zivot;
        this.maxZivot = zivot;
        this.utok = utok;
        this.obrana = obrana;
        this.kocka = kocka;

    }



    @Override
    public String toString(){
        return meno;
    }
    //žije hej? ne?
    public boolean nazive(){
        return (zivot>0);
    }

    public String grafckyZivot(){
        String s = "[";
        int celkom = 20;
        double pocet =Math.round((double) zivot / maxZivot * celkom);
        System.out.println(pocet);
        //ak ma min ziv 0,2 doplní ho do 1
        if(pocet == 0 && nazive()){
            pocet = 1;
        }

        for(int i = 0; i<pocet ; i++){
            s +="#";
        }

        for(int i = 0; i<celkom - pocet ; i++){
            s +=" ";
        }

        s += "]";
        return s;

    }
    public void branSa(int uder){
        int zranenie = uder -(obrana + kocka.hod());
        if (zranenie>0){
            zivot-= zranenie;
            sprava = this.meno + "utrpel zranenie " + zranenie + "hp";
            if(zivot<=0){
                zivot = 0;
                sprava += " zomrel";
            }else{
                sprava = this.meno + "odrazil utok a nestratil žiadne hp";
            }
        }
    }
    public void utoc (Bojovnik nepriatel){
        int uder = utok + kocka.hod();
        nastavSpravu(this.meno + "útočí s úderom za "+ uder+ "hp");
        nepriatel.branSa(uder);
    }

    public void nastavSpravu(String sprava){
        this.sprava = sprava;
    }
    public String vratPoslednuSpravu(){
        return sprava;
    }
}

//kocka
package com.company;
import  java.util.Random;
public class Kocka {
    private Random random;
    private int pocetStien;

    public Kocka(int pocetSten){
        pocetStien = pocetSten;
        random = new Random();
    }

    public Kocka(){
        pocetStien = 6;
        random = new Random();
    }

    public  int vratPocetStien(){
        return pocetStien;
    }
    public int hod(){
        return random.nextInt(pocetStien)+ 1;
    }

}


//Arena
package com.company;

public class Arena {
    private Bojovnik bojovnik1;
    private Bojovnik bojovnik2;

    private Kocka kocka;

    public Arena(Bojovnik bojovnik1, Bojovnik bojovnik2, Kocka kocka){
        this.bojovnik1 = bojovnik1;
        this.bojovnik2 = bojovnik2;
        this.kocka = kocka;
    }
    private void  vykresli(){
        System.out.println("--------------Arena--------------");
        System.out.println("Zdravie bojovníkov: \n");
        System.out.printf("%s %s\n", bojovnik1, bojovnik1.grafckyZivot());
        System.out.printf("%s %s\n", bojovnik2, bojovnik2.grafckyZivot());
    }
    private void vypisPpravu(String sprava){
        System.out.println(sprava);

        try{
            Thread.sleep(1000);
        }catch(InterruptedException ex){
            System.err.println("Chyba");
        }
    }
    public void zapas(){
        System.out.println("Vitaj v arene");
        System.out.println("Dnes sa proti sebe postavia " + this.bojovnik1 + "a"+ this.bojovnik2);
        System.out.println("Zápas môže začať");

        while(bojovnik1.nazive() && bojovnik2.nazive()){
            bojovnik1.utoc(bojovnik2);
            vykresli();
            vypisPpravu(bojovnik1.vratPoslednuSpravu());
            vypisPpravu(bojovnik2.vratPoslednuSpravu());
            bojovnik2.utoc(bojovnik1);
            vypisPpravu(bojovnik1.vratPoslednuSpravu());
            vypisPpravu(bojovnik2.vratPoslednuSpravu());
            System.out.println();
        }

    }



}




//Main
package com.company;

public class Main {

    public static void main(String[] args) {


    Kocka desatStennaKocka = new Kocka(10);
    Kocka sestStennaKocka = new Kocka(6);
    Kocka dvadsatPatStennaKocka = new Kocka(25);
    Bojovnik bojovník  = new Bojovnik("Valach",100,40,10, desatStennaKocka);
    Bojovnik enemy  = new Bojovnik("Kameň",120,15,5, desatStennaKocka);
    Arena arena = new Arena(bojovník, enemy, desatStennaKocka);

    arena.zapas();
    }
}
