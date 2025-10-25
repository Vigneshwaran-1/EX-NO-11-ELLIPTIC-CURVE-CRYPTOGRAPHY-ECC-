# EX-NO-11-ELLIPTIC-CURVE-CRYPTOGRAPHY-ECC

## Aim:
To Implement ELLIPTIC CURVE CRYPTOGRAPHY(ECC)


## ALGORITHM:

1. Elliptic Curve Cryptography (ECC) is a public-key cryptography technique based on the algebraic structure of elliptic curves over finite fields.

2. Initialization:
   - Select an elliptic curve equation \( y^2 = x^3 + ax + b \) with parameters \( a \) and \( b \), along with a large prime \( p \) (defining the finite field).
   - Choose a base point \( G \) on the curve, which will be used for generating public keys.

3. Key Generation:
   - Each party selects a private key \( d \) (a random integer).
   - Calculate the public key as \( Q = d \times G \) (using elliptic curve point multiplication).

4. Encryption and Decryption:
   - Encryption: The sender uses the recipient’s public key and the base point \( G \) to encode the message.
   - Decryption: The recipient uses their private key to decode the message and retrieve the original plaintext.

5. Security: ECC’s security relies on the Elliptic Curve Discrete Logarithm Problem (ECDLP), making it highly secure with shorter key lengths compared to traditional methods like RSA.

## Program:
~~~
#include <stdio.h>

int p = 11; 
int a = 1;
int b = 6;

int modInverse(int k) {
    k = k % p;
    for (int x = 1; x < p; x++) {
        if ((k * x) % p == 1)
            return x;
    }
    return -1;
}

void pointAdd(int x1, int y1, int x2, int y2, int *x3, int *y3) {
    int m;
    if (x1 == x2 && y1 == y2) {
     
        int numerator = (3 * x1 * x1 + a) % p;
        int denominator = (2 * y1) % p;
        m = (numerator * modInverse(denominator)) % p;
    } else {
       
        int numerator = (y2 - y1 + p) % p;
        int denominator = (x2 - x1 + p) % p;
        m = (numerator * modInverse(denominator)) % p;
    }

    *x3 = (m * m - x1 - x2 + p * p) % p;
    *y3 = (m * (x1 - *x3) - y1 + p * p) % p;
    *x3 = (*x3 + p) % p;
    *y3 = (*y3 + p) % p;
}

void scalarMult(int k, int x, int y, int *rx, int *ry) {
    int qx = x;
    int qy = y;
    k--;
    while (k--) {
        pointAdd(qx, qy, x, y, &qx, &qy);
    }
    *rx = qx;
    *ry = qy;
}

int main() {
    int Gx = 2, Gy = 7;

    int a_priv = 2;
    int a_pubx, a_puby;
    scalarMult(a_priv, Gx, Gy, &a_pubx, &a_puby);
    printf("Alice Public Key: (%d, %d)\n", a_pubx, a_puby);

    int b_priv = 3;
    int b_pubx, b_puby;
    scalarMult(b_priv, Gx, Gy, &b_pubx, &b_puby);
    printf("Bob Public Key:   (%d, %d)\n", b_pubx, b_puby);

    int s1x, s1y;
    scalarMult(a_priv, b_pubx, b_puby, &s1x, &s1y);

    int s2x, s2y;
    scalarMult(b_priv, a_pubx, a_puby, &s2x, &s2y);

    printf("\nShared Secret (Alice): (%d, %d)\n", s1x, s1y);
    printf("Shared Secret (Bob):   (%d, %d)\n", s2x, s2y);

    if (s1x == s2x && s1y == s2y)
        printf("\nshared secret matches!\n");
    else
        printf("\n❌Shared secret mismatch!\n");

    return 0;
}
~~~


## Output:
<img width="432" height="208" alt="image" src="https://github.com/user-attachments/assets/91ad98b0-36b0-4f79-881d-1e400bc9ac2c" />


## Result:
The program is executed successfully

