import java.util.*;
import java.io.*;

public class Main {

    static final int MAX = 100;
    static String[] noms = new String[MAX];
    static double[] prix = new double[MAX];
    static int[] qteStock = new int[MAX];
    static int compteur = 0;
    static Scanner sc = new Scanner(System.in);

    static void menu() {
        System.out.println("\n*** Gestion Stock ***");
        System.out.println("1) Ajouter produit");
        System.out.println("2) Voir stock");
        System.out.println("3) Chercher produit");
        System.out.println("4) Vendre produit");
        System.out.println("5) Alertes stock faible");
        System.out.println("6) Sauvegarder");
        System.out.println("7) Quitter");
    }

    static int demanderInt(String msg) {
        System.out.print(msg);
        while (!sc.hasNextInt()) {
            System.out.println("Oups, faut mettre un nombre...");
            sc.next();
        }
        return sc.nextInt();
    }

    static void ajouter() {
        if (compteur >= MAX) {
            System.out.println("Pas de place pour plus de produits !");
            return;
        }

        sc.nextLine(); 
        System.out.print("Nom : ");
        noms[compteur] = sc.nextLine();

        System.out.print("Prix : ");
        prix[compteur] = sc.nextDouble();

        System.out.print("Quantité : ");
        qteStock[compteur] = sc.nextInt();

        compteur++;
        System.out.println("Super ! Produit ajouté ");
    }

    static int chercher(String nom) {
        for (int i = 0; i < compteur; i++) {
            if (noms[i].equalsIgnoreCase(nom)) return i;
        }
        return -1;
    }

    static void voirStock() {
        System.out.println("\nNom | Prix | Qte | Total");
        for (int i = 0; i < compteur; i++) {
            double total = prix[i] * qteStock[i];
            System.out.println(noms[i] + " | " + prix[i] + " | " + qteStock[i] + " | " + total);
        }
        System.out.println("Fin de l'inventaire ");
    }

    static void vendre() {
        sc.nextLine();
        System.out.print("Produit à vendre : ");
        String nom = sc.nextLine();
        int idx = chercher(nom);

        if (idx == -1) {
            System.out.println("Désolé, produit introuvable !");
            return;
        }

        System.out.print("Quantité : ");
        int q = sc.nextInt();

        if (qteStock[idx] < q) {
            System.out.println("Pas assez en stock ! ");
            return;
        }

        double total = prix[idx] * q;
        if (total > 1000) {
            total *= 0.9;
            System.out.println("Remise 10% appliquée !");
        }

        qteStock[idx] -= q;
        System.out.println("Vente faite, total : " + total + " DH");
    }

    static void alerte() {
        for (int i = 0; i < compteur; i++) {
            if (qteStock[i] < 5) {
                System.out.println(" Stock faible : " + noms[i] + " (" + qteStock[i] + ")");
            }
        }
    }

    static void sauvegarder() {
        try (PrintWriter pw = new PrintWriter(new FileWriter("stock.txt"))) {
            for (int i = 0; i < compteur; i++) {
                pw.println(noms[i] + ";" + prix[i] + ";" + qteStock[i]);
            }
            System.out.println("Inventaire sauvegardé !");
        } catch (Exception e) {
            System.out.println("Erreur lors de la sauvegarde !");
        }
    }

    static void charger() {
        try {
            File f = new File("stock.txt");
            if (!f.exists()) return;

            Scanner fs = new Scanner(f);
            while (fs.hasNextLine()) {
                String[] t = fs.nextLine().split(";");
                noms[compteur] = t[0];
                prix[compteur] = Double.parseDouble(t[1]);
                qteStock[compteur] = Integer.parseInt(t[2]);
                compteur++;
            }
            fs.close();
            System.out.println("Inventaire chargé depuis fichier ");
        } catch (Exception e) {
            System.out.println("Pas de fichier à charger...");
        }
    }

    public static void main(String[] args) {
        charger();
        int choix;

        do {
            menu();
            choix = demanderInt("Votre choix ? ");

            switch (choix) {
                case 1: ajouter(); break;
                case 2: voirStock(); break;
                case 3:
                    sc.nextLine();
                    System.out.print("Nom à chercher : ");
                    int res = chercher(sc.nextLine());
                    System.out.println(res != -1 ? "Produit à l'index " + res : "Aucun résultat");
                    break;
                case 4: vendre(); break;
                case 5: alerte(); break;
                case 6: sauvegarder(); break;
                case 7: System.out.println("Merci cher utilisateur !"); break;
                default: System.out.println("Choix invalide "); break;
            }

        } while (choix != 7);

        sauvegarder();
    }
}# sundusdahmani_rkiaelwahidi
