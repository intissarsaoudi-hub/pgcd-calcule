# pgcd-calcule
#include <stdio.h>

int main() {
    int n, i;
    int a, b, r;

    printf("=== TP : PGCD et Equation Diophantienne ===\n");

    // ====== PARTIE 1 : Calcul du PGCD de plusieurs nombres ======
    printf("Combien de nombres voulez-vous entrer ? ");
    scanf("%d", &n);

    if (n < 2) {
        printf("Il faut au moins deux nombres !\n");
        return 0;
    }

    printf("Entrez le premier nombre : ");
    scanf("%d", &a);

    for (i = 2; i <= n; i++) {
        printf("Entrez le nombre %d : ", i);
        scanf("%d", &b);

        int a0 = a, b0 = b;
        while (b != 0) {
            r = a % b;
            a = b;
            b = r;
        }
        printf("→ PGCD(%d, %d) = %d\n", a0, b0, a);
    }

    printf("\n Le PGCD de ces %d nombres est : %d\n", n, a);

    // ====== PARTIE 2 : Equation Diophantienne ax + by = c ======
    printf("\n=== Resolution de l'equation ax + by = c ===\n");
    int A, B, C;
    printf("Entrez les valeurs de a, b et c : ");
    scanf("%d %d %d", &A, &B, &C);

    // Algorithme d’Euclide étendu intégré ici
    int a0 = A, b0 = B;
    int x0 = 1, y0 = 0;
    int x1 = 0, y1 = 1;
    int q, x2, y2;

    while (b0 != 0) {
        q = a0 / b0;
        r = a0 % b0;
        x2 = x0 - q * x1;
        y2 = y0 - q * y1;
        a0 = b0;
        b0 = r;
        x0 = x1;
        y0 = y1;
        x1 = x2;
        y1 = y2;
    }

    int d = a0; // PGCD(A, B)
    int x = x0;
    int y = y0;

    if (C % d != 0) {
        printf("\n Pas de solution entiere car %d ne divise pas %d.\n", d, C);
        return 0;
    }

    int mult = C / d;
    int xp = x * mult;
    int yp = y * mult;
    int alpha = B / d;
    int beta = -A / d;

    printf("\n Il existe des solutions entieres car %d divise %d.\n", d, C);
    printf("Solution particuliere : (x, y) = (%d, %d)\n", xp, yp);
    printf("Solutions generales : (x, y) = (%d + %d*k, %d + %d*k), k ∈ Z\n",
           xp, alpha, yp, beta);

    return 0;
}
