# Optimal-String-Game

Lily and Jack are playing a game with a binary string S of length N and an initially empty string T. Lily goes first, and in her turn, she selects the first character of S, choosing to add it either to the front or back of T, then removes that character from S. Jack, on his turn, picks the last character of S, appending it to either the front or back of T, then deletes it from S. They continue until S is empty. Lily aims to make T as lexicographically small as possible, while Jack wants T to be as lexicographically large as possible. What will be the final string T if both play optimally?

Input
The first line of input will contain a single integer T, denoting the number of test cases.
Each test case consists of multiple lines of input.
The first line of each test case contains a single integer N - the length of the string S.
The next line is the binary string S.

Constraints
1 ≤ T ≤ 100
1 ≤ N ≤ 1000
S can only contain the characters 0 or 1.
Output
For each test case, print the the resultant string T, if both of them play optimally.
Example
Sample Input
2
2
10
6
010111
Sample Output
10
101101
Explanation
Test case 1: Lily picks the first bit which is 1 and appends it to the empty string T. Jack then picks the last bit 0 and appends it to the back of T making the resultant string to be 10.

from collections import deque

def optimal_string_game(test_cases):
    results = []
    
    for N, S in test_cases:
        T = deque()
        left, right = 0, N - 1
        turn_lily = True  # Lily starts
        
        while left <= right:
            if turn_lily:
                # Lily's turn: Minimize lexicographical order
                if T:
                    if S[left] + "".join(T) < "".join(T) + S[left]:
                        T.appendleft(S[left])
                    else:
                        T.append(S[left])
                else:
                    T.append(S[left])
                left += 1
            else:
                # Jack's turn: Maximize lexicographical order
                if T:
                    if S[right] + "".join(T) > "".join(T) + S[right]:
                        T.appendleft(S[right])
                    else:
                        T.append(S[right])
                else:
                    T.append(S[right])
                right -= 1
            
            # Switch turn
            turn_lily = not turn_lily
        
        results.append("".join(T))
    
    return results

# Input and output handling
if __name__ == "__main__":
    T = int(input())
    test_cases = []
    for _ in range(T):
        N = int(input())
        S = input().strip()
        test_cases.append((N, S))
    
    results = optimal_string_game(test_cases)
    for res in results:
        print(res)
