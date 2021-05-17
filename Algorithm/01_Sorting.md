## 정렬 알고리즘



## 버블정렬 - O(N^2)

~~~c++
void bubble_sort(vector<int> & list){
  for (int i = 0 ; i < list.size() ; i++){
    for (int j = 0 ; j < list.size() - i - 1 ; j++){
      if(list[j] > list[j + 1]){
        swap (list[j], list[j + 1]);
      }
    }
  }
}
~~~

- 로직(오름차순)
  - 처음부터 n - i 까지 인접한 인덱스를 비교해서 값이 가장 큰 요소를 n - i 번째에 위치시킵니다.
  - i는 정렬이 완료된 요소의 개수를 의미합니다.
- 시간복잡도 
  - N 개의 인덱스에 대해 최악의 경우 N 번 비교연산을 수행해야하기 때문에 O(N^2) 의 시간복잡도를 가집니다.
- 정렬 형태
  - 안정 정렬입니다.



## 선택정렬 - O(N^2)

~~~c++
void selection_sort (vector<int> & list){
  for (int i = 0 ; i < list.size() ; i++){
    for (int j = 0 ; j < list.size() ; j++){
      if (list[i] < list[j]){
        swap(list[i], list[j]);
      }
    }
  }
}
~~~

- 로직(오름차순)

  - 첫 인덱스부터 다른 모든 인덱스를 비교하면서 자신보다 작은 값이 나오면 자리를 교환해줍니다. 

- 시간 복잡도

  - 정렬 여부와 상관없 모든 인덱스와의 대소비교를 해야하므로 O(N^2) 의 시간복잡도를 가집니다.

- 정렬 형태

  - 불안정 정렬입니다.

  

## 삽입정렬 - O(N^2)

~~~c++
void insertion_sort (vector<int> & list){
  int i, j;
	for (i = 1 ; i < list.size() ; i++){
    int current = list[i];
    for (j = i - 1 ; j >= 0 ; j--){
      if(list[j] > current){
        list[j + 1] = list[j];
      }
      else{
        break;
      }
    }
    
    list[j + 1] = current;
  }
}
~~~

- 로직
  - 1번째 인덱스에서 시작해서 키 값을 선택해 자신보다 앞에 있는 인덱스들의 값이 키보다 더 크다면 해당 값을 한칸 뒤로 밀어줍니다. 키 보다 작은 값을 가진 인덱스를 만나면 이동을 멈추고 키 값을 빈 자리에 넣어줍니다.
- 시간 복잡도
  - 만약 배열이 이미 정렬되어 있다면 한 번의 비교만으로 끝나기 때문에 O(N) 이지만 최악의 경우에는 모든 키 값에 대해 모든 인덱스를 비교해야하기 때문에 O(N^2)가 시간복잡도가 됩니다.
- 정렬 형태
  - 안정 정렬입니다.

## 병합정렬 - O(NlogN)

~~~c++
void merge (vector<int> & list, int left, int mid, int right){
  vector<int> sorted_list(list.size());
  int i = left, j = mid+1, idx = left;
  while(i <= mid && j <= right){
    if (list[i] <= list[j]){
      sorted_list[idx++] = list[i++];
    }
    else{
      sorted_list[idx++] = list[j++];
    }
  }
  while(i <= mid){
    sorted_list[idx++] = list[i++];
  }
  
  while(j <= right){
    sorted_list[idx++] = list[j++];
  }
  
  for (int l = left ; l <= right ; l++){
    list[l] = sorted_list[l];
  }
}

void partition (vector<int> & list, int left, int right){
  int mid;
  if (left < right){
    mid = (left + right) / 2;
    partition(list, left, mid);
    partition(list, mid+1, right);
    merge(list, left, mid, right);
  }
}
~~~

- 로직 
  - 분할정복으로 배열을 반으로 계속 분할해서 분할된 부분배열을 정렬하고 다시 합쳐줍니다.

- 시간 복잡도
  - 배열을 계속해서 반으로 나누어 정렬하기 때문에 재귀호출의 깊이에 따라 O(NlogN) 의 시간복잡도를 가집니다.
- 정렬 형태
  - 안정 정렬입니다.



## 퀵 정렬 - O(NlogN)

~~~c++
void quick_sort (vector<int> & list, int head, int tail){
  if (head >= tail) return;
  
  int pivot = head;
  int left = pivot + 1;
  int right = tail;
  
  while(left <= right){
    while(left <= tail && list[left] <= list[pivot]){
      left++;
    }
    while(right > head && list[right] >= list[pivot]){
      right--;
    }
    
    if (left > right){
      swap(list[pivot], list[right]);
    }
    else{
      swap(list[right], list[left]);
    }
  }
  
  quick_sort(list, head, right - 1);
  quick_sort(list, right + 1, tail);
} 
~~~

- 로직
  - 배열의 첫 값을 피봇으로 지정하고 1번째 인덱스부터 뒤로 이동하며 피봇보다 큰 값이 나올 때까지 left를 이동합니다.
  - 동시에 배열에 마지막 인덱스에서부터 앞으로 이동하며 피봇보다 작은 값이 나올때까지 right를 이동합니다. 
  - 이동이 끝났을 때, left 가 right를 추월하지 않았다면 left 와 right의 자리를 바꿔줍니다.
  - 이동이 끝났을 때, left 가 right 를 추월해 있다면 pivot 과 right 의 자리를 바꿔줍니다.
  - 한 피봇에 대해 정렬이 끝나면 해당 피봇을 기준으로 오른쪽과 왼쪽 부분배열을 만들어 같은 알고리즘의 정렬을 수행합니다.
  - 나누는 부분배열의 길이가 1이나 0이 될 때까지 반복합니다.
- 시간 복잡도
  - 시간복잡도는 재귀호출의 깊이에 따라 O(NlogN)이 됩니다. 하지만 최악의 경우에는 피봇이 중앙에 위치하지 않게 되어 불균형하게 배열이 나누어지고 이 때는 O(N^2) 의 시간복잡도가 됩니다.
- 정렬 형태
  - 불안정 정렬입니다.