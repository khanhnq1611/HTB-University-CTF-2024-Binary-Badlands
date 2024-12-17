``` C++
#include <bits/stdc++.h>
using namespace std;

int main() {
    string n;
    getline(cin, n);

    // Split the input into integers
    stringstream ss(n);
    int num, ma=0;
    vector<int> array;
    while (ss >> num) {
        array.push_back(num);
        if (ma < num) ma = num;
    }

    // Use a map to count occurrences
    map<int, int> frequency_map;
    for (int i = 0; i < array.size(); i++) {
        frequency_map[array[i]]++;
        if (frequency_map[array[i]] == 1) cout<<array[i]<<" ";
    }


    return 0;
}
```
