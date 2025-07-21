```c
#include <stdio.h>
#include <stdbool.h>
#include <string.h>
#include <ctype.h>

typedef struct {
    int state1;
    char transition[5];
    int state2;
} Transition;

Transition transitions[] = {
    {0, "sig", 1},
    {0, "exp", 999},
    {0, "deg", 3},
     {0, ".", 5},
    
    {1, "sig", 999},
    {1, "exp", 999},
    {1, "deg", 4},
    {1, ".", 5},
    
    {3, "sig", 999},
    {3, "exp", 6},
    {3, "deg", 3},
    {3, ".", 7},

    {4, "sig", 999},
    {4, "exp", 6},
    {4, "deg", 4},
    {4, ".", 7},

    {5, "deg", 7},
    {5, "exp", 999},
    {5, ".", 999},

    {6, "sig", 8},
    {6, "deg", 9},

    {7, "deg", 7},
    {7, "exp", 6},

    {8, "deg", 9},

    {9, "deg", 9}
};

int numTransitions = sizeof(transitions) / sizeof(transitions[0]);

const char* classifyChar(char c) {
    if (c == '+' || c == '-') return "sig";
    if (isdigit(c)) return "deg";
    if (c == 'e' || c == 'E') return "exp";
    if (c == '.') return ".";
    return NULL;
}

bool isNumber(char* s) {
    int currentState = 0;

    for (int i = 0; s[i] != '\\0'; i++) {
        const char* token = classifyChar(s[i]);
        if (!token) return false;

        bool matched = false;
        for (int j = 0; j < numTransitions; j++) {
            if (transitions[j].state1 == currentState &&
                strcmp(transitions[j].transition, token) == 0) {
                currentState = transitions[j].state2;
                matched = true;
                break;
            }
        }

        if (!matched || currentState == 999) return false;
    }

    return currentState == 3 || currentState == 4 || currentState == 7 || currentState == 9;
}

```