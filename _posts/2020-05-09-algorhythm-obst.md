---
layout: post
title:  "Algo-Rhythm optimal binnary search tree (최적이진트리)"
date:   2020-05-09
excerpt: "이진 트리 구성을 최적으로 하는 법과 DP(Dynamic Programming)"
tag:
- c++
- algorithm
- algo-rhythm
- bst
- binary search tree
- dp
- dynamic programming
- blog
- silofox
comments: true
lastmod : 2020-05-09
sitemap : 
  changefreq : never
  priority : 1.0
feature: https://github.com/siloFoX/siloFoX.github.io/blob/master/images/algo-rhythm/algo-rhythm-feature.jpg?raw=true
---

## 미완 Optimal Binary Search Tree

source
```c++
#include <iostream>
#include <fstream>
#include <cstring>

using namespace std;

float optimal_value[101][102];
int optimal_idx[101][102];

void calculate_optimal_value(int num_of_keys);
void build_tree(int i, int j);

int main() {

    fstream fin("input.txt");

    int num_of_data;
    fin >> num_of_data;

    while(num_of_data--) {

        int num_of_keys;
        fin >> num_of_keys;

        memset(optimal_value, 0, sizeof(float) * 101 * 102);
        memset(optimal_idx, 0, sizeof(int) * 101 * 102);

        int sum_of_key_frequency = 0;
        int idx_key;
        for(idx_key = 0; idx_key < num_of_keys; idx_key++) {

            int tmp;
            fin >> tmp;

            optimal_value[idx_key][idx_key + 1] = 0;
            optimal_value[idx_key + 1][idx_key + 1] = tmp;
            optimal_idx[idx_key + 1][idx_key + 1] = idx_key + 1;
            optimal_idx[idx_key][idx_key + 1] = 0;

            sum_of_key_frequency += tmp;
        }

        for(idx_key = 0; idx_key < num_of_keys; idx_key++)
            optimal_value[idx_key + 1][idx_key + 1] /= sum_of_key_frequency;

        calculate_optimal_value(num_of_keys);
        build_tree(1, num_of_keys);
        cout << endl;
    }


    return 0;
}

void calculate_optimal_value (int num_of_keys) {

    int diagonal;
    for (diagonal = 1; diagonal < num_of_keys; diagonal++) {
        
        int i;
        for (i = 1; i < num_of_keys - diagonal + 1; i++) {

            int j = i + diagonal;

            float p = 0;
            float min_val = 9999;
            int min_idx;
            int k;
            for (k = i; k < j + 1; k++) {
                
                p += optimal_value[k][k];
                if((optimal_value[i][k-1] + optimal_value[k+1][j]) < min_val) {

                    min_val = optimal_value[i][k-1] + optimal_value[k+1][j];
                    min_idx = k;
                }
            }
            
            optimal_idx[i][j] = min_idx;
            optimal_value[i][j] = min_val + p;
        }
    }
}

void build_tree (int i, int j) {

    int k = optimal_idx[i][j];

    if(k == 0)
        return;
    else {

        cout << k << " ";

        build_tree(i, k-1);
        build_tree(k+1, j);
    }
}
```