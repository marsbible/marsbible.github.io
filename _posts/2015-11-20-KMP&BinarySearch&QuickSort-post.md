---
layout: post
title: KMP+QuickSort+BinarySearch三件套，面试必备 
modified: 2015-11-20
tags: [算法]
---

### KMP实现
{% highlight c %}
void preKMP_CLRS(char *P){
    int j, k;
    int pLen = strlen(P);
 
    next[0] = 0;
    for(j=1; j<pLen; j++){
        k = next[j-1];
        while(k > 0 && P[j] != P[k])
            k = next[k-1];        //下标从0开始，所以长度 = 下标 +1
        if(P[j] == P[k])
            k++;
        next[j] = k;
    }
}
 
 
int KMP_CLRS(char *T, char *P, int start){
    int i, j;
    int tLen = strlen(T);
    int pLen = strlen(P);
 
    j = 0;
    for(i=start; i<tLen; i++){
        while(j > 0 && T[i] != P[j]){
            j = next[j-1];
        }
        if(T[i] == P[j])
            j++;
        if(j == pLen)
            return i - j + 1;
    }
    return -1;
}
 
//找所有子串
void KMP_CLRS_2(char *T, char *P, int start){
    int i, j;
    int tLen = strlen(T);
    int pLen = strlen(P);
 
    j = 0;
    for(i=start; i<tLen; i++){
        while(j > 0 && T[i] != P[j]){
            j = next[j-1];
        }
        if(T[i] == P[j]){
            if(j == pLen - 1){
                cout << i - j << endl;
                j = next[j];
                j--;        //注意：长度 = 下标 + 1
            }
            j++;
        }
    }
}
{% endhighlight %}

### QuickSort实现
{% highlight c %}
void quicksort(Item a[], int l, int r)
{
    int i = l-1, j = r;
    Item v = a[r];
    if (r <= l) return;
    for (;;)
    {
        while (a[++i] < v) ;
        while (v < a[--j]) if (j == l) break;
        if (i >= j) break;
        exch(a[i], a[j]);
    }
    exch(a[i], a[r]);
    quicksort(a, l, i-1);
    quicksort(a, i+1, r);
}
{% endhighlight %}

### BinarySearch实现
{% highlight c %}
int binary_search(int[] A, int key, int imin, int imax)  
{
  // continue searching while [imin,imax] is not empty
  while (imin <= imax)
  {
	  // calculate the midpoint for roughly equal partition
	  int imid = imin + ((imax - imin) / 2);
	  if(A[imid] == key)
		// key found at index imid
		return imid; 
	  // determine which subarray to search
	  else if (A[imid] < key)
		// change min index to search upper subarray
		imin = imid + 1;
	  else         
		// change max index to search lower subarray
		imax = imid - 1;
  }
  // key was not found
  return -1;
}
{% endhighlight %}