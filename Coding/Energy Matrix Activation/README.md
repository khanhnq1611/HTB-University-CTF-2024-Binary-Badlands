


```C++
#include <bits/stdc++.h>
using namespace std;

int main() {
    string energy_crystals, target_energy;
    getline(cin, energy_crystals); 
    getline(cin, target_energy);   

    // Remove the brackets from the energy_crystals string
    energy_crystals = energy_crystals.substr(1, energy_crystals.size() - 2);

    // Parse energy_crystals into a vector of integers
    vector<int> crystals;
    stringstream ss(energy_crystals);
    string num;
    while (getline(ss, num, ',')) {
        crystals.push_back(stoi(num));
    }

    int target = stoi(target_energy);

    vector<int> dp(target + 1, 0);
    dp[0] = 1; // Base case: 1 way to make energy level 0

    // Fill the dp array
    for (int crystal : crystals) {
        for (int j = crystal; j <= target; ++j) {
            dp[j] += dp[j - crystal];
        }
    }
    cout << dp[target] << endl;
    return 0;
}
// HTB{3n34gy_m4tr1x_act1v4t3d_w3_4r3_s4v3d!_1015195f5fdd75455de435aec0fb24af}
```
