#include <stdio.h>
#include <stdlib.h>

/* PGCD de deux nombres */
int pgcd(int a, int b) {
    if (b == 0)
        return abs(a);
    return pgcd(b, a % b);
}

/* PGCD de plusieurs nombres */
int pgcd_multiple(int tab[], int n) {
    int res = tab[0];
    for (int i = 1; i < n; i++) {
        res = pgcd(res, tab[i]);
        if (res == 1)
            return 1;
    }
    return res;
}

/* PPCM de deux nombres */
int ppcm(int a, int b) {
    return abs(a * b) / pgcd(a, b);
}

/* Algorithme d’Euclide étendu (Bézout) */
int bezout(int a, int b, int *x, int *y) {
    if (b == 0) {
        *x = 1;
        *y = 0;
        return a;
    }
    int x1, y1;
    int d = bezout(b, a % b, &x1, &y1);
    *x = y1;
    *y = x1 - (a / b) * y1;
    return d;
}

/* Équation diophantienne ax + by = c */
void equation_diophantienne(int a, int b, int c) {
    int x0, y0;
    int d = bezout(a, b, &x0, &y0);

    if (c % d != 0) {
        printf("Aucune solution.\n");
        return;
    }

    x0 *= c / d;
    y0 *= c / d;

    printf("Solution particulière : x = %d, y = %d\n", x0, y0);
    printf("Solutions générales :\n");
    printf("x = %d + %d*k\n", x0, b / d);
    printf("y = %d - %d*k\n", y0, a / d);
}

/* Test nombre premier */
int est_premier(int n) {
    if (n <= 1)
        return 0;
    for (int i = 2; i * i <= n; i++) {
        if (n % i == 0)
            return 0;
    }
    return 1;
}

/* Programme principal */
int main() {
    int choix, a, b, c, n;

    do {
        printf("\n===== MENU =====\n");
        printf("1. PGCD de plusieurs nombres\n");
        printf("2. PPCM de deux nombres\n");
        printf("3. Equation de Bezout\n");
        printf("4. Equation diophantienne ax + by = c\n");
        printf("5. Test nombre premier\n");
        printf("0. Quitter\n");
        printf("Choix : ");
        scanf("%d", &choix);

        switch (choix) {

        case 1: {
            printf("Combien de nombres ? ");
            scanf("%d", &n);

            int tab[n];
            printf("Entrer les nombres :\n");
            for (int i = 0; i < n; i++)
                scanf("%d", &tab[i]);

            printf("PGCD = %d\n", pgcd_multiple(tab, n));
            break;
        }

        case 2:
            printf("Entrer a et b : ");
            scanf("%d %d", &a, &b);
            printf("PPCM(%d, %d) = %d\n", a, b, ppcm(a, b));
            break;

        case 3: {
            int x, y;
            printf("Entrer a et b : ");
            scanf("%d %d", &a, &b);
            int d = bezout(a, b, &x, &y);
            printf("PGCD = %d\n", d);
            printf("Coefficients de Bezout : x = %d, y = %d\n", x, y);
            break;
        }

        case 4:
            printf("Entrer a, b et c : ");
            scanf("%d %d %d", &a, &b, &c);
            equation_diophantienne(a, b, c);
            break;

        case 5:
            printf("Entrer un nombre : ");
            scanf("%d", &a);
            if (est_premier(a))
                printf("%d est un nombre premier.\n", a);
            else
                printf("%d n'est pas un nombre premier.\n", a);
            break;

        case 6:
            printf("Fin du programme.\n");
            break;

        default:
            printf("Choix invalide.\n");
        }

    } while (choix != 0);

    return 0;
}
