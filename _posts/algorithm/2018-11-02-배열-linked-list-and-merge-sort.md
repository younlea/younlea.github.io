---
title: "배열 linked list and merge sort"
excerpt_separator: "<!--more-->"
date: 2018-11-02 08:44:03 +0900
categories:
  - algorithm
tags:
  - algorithm
  - 알고리즘

toc : true
toc_sticky : true
---

merge sort

|  |
| --- |
| void merge\_sort(int l, int r){  if (r - l < 1) return;  int half = (l + r) / 2, idx = l, li = l, ri = half + 1;  merge\_sort(l, half);  merge\_sort(half + 1, r);  while (li <= half && ri <= r){   if (W[li].e < W[ri].e) m\_b[idx++] = li++;   else m\_b[idx++] = ri++;  }  while (li <= half) m\_b[idx++] = li++;  while (ri <= r) m\_b[idx++] = ri++; |

binary search

|  |
| --- |
| void binary\_search(int low, int high, int target) {  int mid;   if (low > high)  {   return;  }   mid = (low + high) / 2;   if (target < buff\_sort[mid].t)  {   binary\_search(low, mid - 1, target);  }  else if (buff\_sort[mid].t < target)  {   binary\_search(mid + 1, high, target);  }  else  {   cc = mid;   return;  } } |

linked list

|  |
| --- |
| struct Node{  int to, cost;  Node \*next; };  int nidx; Node nodes[1000001]; |

linked list 문제 (disk scheduling)

|  |
| --- |
| #define DIV 100  #define LEFTMIN -200000 #define RIGHTMAX 300000  typedef struct Node{  int data;  int next; };  int Head[100000 / DIV + 5]; Node List[100005]; int queue[100005], front, rear; int N, H, M;  bool flag[100005]; bool dir;  void add(int val){  List[M].data = val;  List[M].next = Head[val / DIV];  Head[val / DIV] = M++; }  void remove\_list(int val) {  int prev = 0;  int cur = Head[val / DIV];   while (cur){   if (List[cur].data == val){    if (prev == 0){     Head[val / DIV] = List[cur].next;    }    else{     List[prev].next = List[cur].next;    }    break;   }   prev = cur;   cur = List[cur].next;  } }  int findLeft(int val){  int max = LEFTMIN;   for (int i = val / DIV; i >= 0 && max == LEFTMIN; --i){   int cur = Head[i];    while (cur){    if (List[cur].data <= val){     if (max < List[cur].data){      max = List[cur].data;     }    }    cur = List[cur].next;   }  }  return max; }  int findRight(int val){  int min = RIGHTMAX;   for (int i = val / DIV; i <= N / DIV && min == RIGHTMAX; ++i){   int cur = Head[i];    while (cur){    if (List[cur].data >= val){     if (min > List[cur].data){      min = List[cur].data;     }    }    cur = List[cur].next;   }  }  return min; }  int init(int track\_size, int head){  N = track\_size;  H = head;  M = 1;  dir = false;  front = rear = 0;   for (int i = 0; i < (100000 / DIV + 5); ++i){   Head[i] = 0;  }   for (int i = 0; i <= track\_size; ++i){   flag[i] = false;  } }  void request(int track){  queue[rear++] = track;  add(track); }  int fcfs(){  int qValue;   while (front < rear){   qValue = queue[front++];    if (!flag[qValue]){    break;   }  }   flag[qValue] = true;  remove\_list(qValue);   return H = qValue; }  int sstf(){  int left = findLeft(H);  int right = findRight(H);   if (H - left <= right - H){   H = left;  }  else{   H = right;  }  flag[H] = true;  remove\_list(H);   return H; }  int look(){  int left = findLeft(H);  int right = findRight(H);   if (!dir){   if (left == LEFTMIN){    dir = true;    H = right;   }   else H = left;  }  else{   if (right == RIGHTMAX){    dir = false;    H = left;   }   else H = right;  }  flag[H] = true;  remove\_list(H);   return H; }  int clook(){  int left = findLeft(H);   if (left == LEFTMIN){   left = findLeft(N);  }   H = left;  flag[H] = true;  remove\_list(H);   return H; } |