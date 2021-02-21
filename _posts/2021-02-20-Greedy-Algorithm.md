---
layout: post
title: Thuật toán tham lam

tags: [Greedy, Algorithm]
comments: true
---

## Thuật toán tham lam là gì
<br>
Thuật toán tham lam hay chính xác hơn là một kĩ thuật (technique) tương tự quy hoạch động hay chia để trị cũng là những kĩ thuật luôn chọn quyết định tốt nhất ở thời điểm hiện tại hay lựa chọn tối ưu cục bộ và hy vọng rằng quyết định đó sẽ dẫn tới giải pháp tối ưu toàn cục của bài toán. Do đó nó khác với quy hoạch động luôn duyệt hết và đảm bảo tìm thấy lời giải, với quy hoạch động mọi quyết định đều phải dựa vào quyết định của bài toán con đã được giải ở bước trước đó và có thể xét lại đường đi của bước trước hướng tới lời giải. Giải thuật tham lam quyết định sớm và thay đổi đường đi thuật toán theo quyết định đó, và không bao giờ xét lại các quyết định cũ. Chính vì vậy trong nhiều bài toán tham lam có thể không cho kết quả chính xác nhất.<br>
Tham lam không dựa vào quyết định của bài toán trước đó để đưa ra quyết định mà tại mỗi bước nó tự ra quyết định tối ưu cục bộ và quyết định này sẽ quyết định đường đi của mọi bước tiếp theo, quyết định này cũng không thể quay lại hay phục hồi.<br>

https://vi.wikipedia.org/wiki/Gi%E1%BA%A3i_thu%E1%BA%ADt_tham_lam
<br>

**Ưu điểm của tham lam**

* Thuật toán tham lam dễ dàng tiếp cận với các bài toán đặc biệt là các bài toán đồ thị và NP-đầy đủ mặc dù không đảm bảo tìm ra được lời giải tối ưu tuy nhiên nó vẫn trở nên rất hữu ích bởi việc dễ dàng thiết kế để cho ra lời giải ước lượng (tương đối chính xác).
* Độ phức tạp của tham lam là khá rõ ràng từ đó bạn có thể xem xét độ phù hợp của tham lam đối với thời gian chạy của bài toán cần giải quyết.
* Nếu có thể chứng minh rằng một thuật toán tham lam cho ra kết quả tối ưu toàn cục cho một lớp bài toán nào đó, thì thuật toán thường sẽ trở thành phương pháp được chọn lựa, vì nó chạy nhanh hơn các phương pháp tối ưu hóa khác như quy hoạch động.

**Nhược điểm** 

* Rất khó để hiểu hay chứng minh 1 lời giải tham lam là kết quả tối ưu ngay kể cả khi nó đưa ra lời giải tối ưu
* Đa số các thuật toán tham lam là không chính xác.

# Các bài toán tham lam
## Bài toán trồng cây
**Problem:** Một nông dân đang muốn trồng hoa vào khu vườn của mình. Để cho khu vườn trở nên thật màu sắc ông quyết định trồng nhiều loài hoa khác nhau vào khu vườn. Mỗi loài hoa có 1 cách trồng khác nhau do đó ông sẽ trồng từng loài hoa vào các ngày liên tiếp nhau. Cháu của ông rất mong chờ được thấy tất cả loài hoa trong khu vườn đều nở hoa trông sẽ tuyệt vời như thế nào. Tuy nhiên mỗi loài hoa lại có thời gian phát triển từ lúc trồng tới lúc nở hoa khác nhau. Nhiệm vụ của bạn là giúp ông nông dân tìm ra ngày sớm nhất mà tất cả loài hoa đều nở hoa. <br>
Trong kho của ông nông dân hiện tại có một số loài hoa


| | Loài hoa | Thời gian hoa sẽ nở |
| -------- | -------- | -------- |
| 1    | Hoa hồng     | 3     |
| 2    | Hoa lan     | 4     |
| 3    | Hoa cúc    | 2     |
| 4    | Hoa mười giờ     | 1    |

<br>

**Idea:** Mỗi cách trồng là 1 hoán vị của N (N là số loài hoa) với mỗi cách trồng sẽ cần một thời gian chờ để tất cả loài hoa đều nở. Ví dụ<br> Với hoán vị 1234: <br> 
* Trồng hoa hồng (1) vào ngày thứ 1 => sau 1 + 3 = 4 ngày hoa sẽ nở
* Trồng hoa lan (2) vào ngày thứ 2 => sau 2 + 4 = 6 ngày hoa sẽ nở
* Trồng hoa cúc (3) vào ngày thứ 3 => sau 3 + 2 = 5 ngày hoa sẽ nở
* Trồng hoa mười giờ (1) vào ngày thứ 4 => sau 4 + 1 = 5 ngày hoa sẽ nở

Thời gian sớm nhất tất cả loài hoa đều nở là ngày thứ 7 <br>
Với hoán vị 2134: <br>
Thời gian sớm nhất tất cả loài hoa đều nở là ngày thứ 6 <br>
Dễ thấy với các loài hoa có thời gian phát triển lâu cần được trồng trước các loài hoa phát triển nhanh (tham lam)  => Sort theo thứ tự giảm dần thời gian phát triển của hoa để sinh ra hoán vị tốt nhất.

**Implementation**
```C
    sort(a, a+n);
    int res = 1, j = 1;
    for (int i = n-1; i >= 0; i--)  
    {
        a[i] += j++;
        if (res < a[i])
            res = a[i];
    }
    res++;
    printf("%d\n", res);
```


Ngoài ra, còn có 1 số bài toán tham lam tương tự dạng này nữa: <br>
https://www.hackerrank.com/challenges/angry-children/problem <br>
https://www.hackerrank.com/challenges/greedy-florist/problem <br>

## Bài toán sắp xếp công việc
**Problem:** Ngày CN, Minh có rất nhiều dự định muốn hoàn thành chẳng hạn như làm bài tập lớn môn A, viết bài viblo, ôn tập môn B, nghe nhạc, chạy bộ, ... Mỗi công việc đều có thời gian lý tưởng để bắt đầu và kết thúc của nó. Tuy nhiên có một số công việc không thể cùng hoàn thành bởi công việc trước chưa kết thúc công việc sau đã bắt đầu. Chẳng hạn


| Task | start | end |
| -------- | -------- | -------- |
| làm btl môn A     | 8 (h)    | 10 (h)    |
| viết bài viblo     | 9     | 11     |
| ôn tập môn B     | 10     | 12     |
| nghe nhạc     | 14     | 15     |
| chạy bộ     | 17     | 18     |

<br>
Chúng ta không thể hoàn thành cả 2 task là viết bài viblo và làm btl môn A hay ôn tập môn B. Nếu chọn thực hiện viết bài viblo bạn chỉ có thể hoàn thành 3/5 task trong khi nếu không chọn viết bài viblo bạn có thể hoàn thành 4/5 task.<br>
Chúng ta cần giúp Minh tìm ra được số task có thể hoàn thành nhiều nhất mỗi khi ngày CN tới.<br>

**Idea:**
Dễ thấy để chọn được ra nhiều task nhất ta sẽ chọn ra task có thời gian kết thúc sớm nhất và đảm bảo thời gian bắt đầu của nó muộn hơn thời gian kết thúc của các task được chọn trước đó. Riêng với task đầu tiên ta chỉ cần chọn ra task kết thúc sớm nhất.<br>
Ý tưởng tham lam này cũng đã được chứng minh là đúng trong mọi TH tuy nhiên mình nhớ được link để trích dẫn.

**Implementation:**
```C 
    #define MAX 100009
    int s[MAX], e[MAX];
    int N;

    void swap(int i, int j)
    {
        int tmp = e[i];
        e[i] = e[j];
        e[j] = tmp;
        tmp = s[i];
        s[i] = s[j];
        s[j] = tmp;
    }

    void qsort(int a[], int start, int end)
    {
        // quicksort theo thời gian kết thúc
        if (start >= end)
            return;
        int index = rand() % (start-end) + start;
        int pivot = a[index];
        int k = start - 1;
        swap(index, end);
        for (int i = start; i < end; i++)
        if (a[i] < pivot)
        {
            k++;
            swap(i, k);
        }
        k++;
        swap(k, end);
        qsort(a, start, k-1);
        qsort(a, k+1, end);
    }

    void proc()
    {
        // xử lí
        int res = 1;
        int end = e[0];
        for (int i = 1; i < N; i++)
        {
            if (e[i] == end)
                continue;
            if (s[i] >= end)
            {
                res++;
                end = e[i];
            }
        }
        printf("%d\n", res);
    }
    int main()
    {
            scanf("%d", &N);
            for (int i = 0; i < N; i++)
                scanf("%d %d", &s[i], &e[i]); //nhập vào thời bắt đầu và kết thúc của task i
            qsort(e, 0, N-1); // sort theo thời gian kết thúc
            proc(); // xử lí 
        }
    }
```
<br>

Ngoài ra, còn có 1 số bài toán tham lam tương tự dạng này kết hợp với disjoin set nữa: <br>
https://www.geeksforgeeks.org/job-sequencing-problem-set-1-greedy-algorithm/ <br>


## Bài toán ATM withdrawal
Đây là link gốc của bài toán:
http://codeforces.com/problemset/gymProblem/100541/C <br>
Đây là lời dịch tiếng viết và solution <br>

Vinh làm việc cho 1 nhà máy sản xuất cây ATM. Chức năng cơ bản của cây ATM đó là rút tiền. Khi người dùng yêu cầu rút W VND, cây ATM sẽ trả ra N tờ tiền có tổng đúng bằng W. Ở thế hệ tiếp theo của cây ATM, Vinh phải làm việc với thuật toán để tối ưu số tờ tiền trả ra cho mỗi giao dịch là nhỏ nhất.

Nhiệm vụ của bạn là giúp Vinh thực hiện công việc của anh ý với các tờ tiền được đưa vào có giá trị 
1000,  2000,  3000,  5000, 1000 * 10<sup>1</sup>,  2000 * 10<sup>1</sup>,  3000 * 10<sup>1</sup>,  5000 * 10<sup>1</sup>, ...,  1000 * 10<sup>c</sup>,  2000 * 10<sup>c</sup>,  3000 * 10<sup>c</sup>,  5000 * 10<sup>c</sup>. Với c là số nguyên dương.<br>

**Với dữ liệu đầu vào** là 2 số W ( W <= 10<sup>18</sup>) và c (c <= 15). <br>
**Và đầu ra** là 2 số N và S trong đó S là số cách để trả ra số tờ tiền ít nhất N sao cho tổng bằng W. Nếu không thể trả ra W thì chương trình trả về 0.<br>

**Idea:**
Dễ thấy đơn vị nhỏ nhất để thanh toán là 1000. Do đó nếu W không chia hết cho 1000 thì cây ATM không thể thanh toán => Bài toán trở thành dùng các tờ 1,  2,  3,  5, 1 * 10<sup>1</sup>, 2 * 10<sup>1</sup>, 3 * 10<sup>1</sup>, 5 * 10<sup>1</sup>, ..., 1 * 10<sup>c</sup>, 2 * 10<sup>c</sup>, 3 * 10<sup>c</sup>, 5 * 10<sup>c</sup> để trả ra W2  = W/1000. Ý tưởng tham lam đầu tiên đó nếu W lớn ta ưu tiên sử dụng các tờ mệnh giá lớn trước tức đầu tiên nếu W > 1 * 10<sup>c</sup> ta sẽ dùng các tờ 1 * 10<sup>c</sup>, 2 * 10<sup>c</sup>, 3 * 10<sup>c</sup>, 5 * 10<sup>c</sup> trước, sau đó tới các tờ 1 * 10<sup>c-1</sup>, 2 * 10<sup>c-1</sup>, 3 * 10<sup>c-1</sup>, 5 * 10<sup>c-1</sup> và tới c-2, ... 0. <br>
Ví dụ <br>
Với số tiền W = xyzt000 => W2= xyzt và c = 2 ta sẽ làm như sau<br>
* Dùng các tờ 100, 200, 300, 500 để trả ra xy00 đồng => dùng các tờ 1, 2, 3, 5 trả ra xy đồng **(1)**
* Dùng các tờ 10, 20, 30, 50 trả ra z0 đồng => dùng các tờ 1, 2, 3 , 5 trả ra z đồng ( z &isin; [0, 9]) **(2)**
* dùng các tờ 1, 2, 3 , 5 trả ra t đồng ( t &isin; [0, 9]) **(3)**
 
Giả sử mỗi trường hợp (TH)  **(1), (2), (3)**  ta cần lấy số tờ ít nhất là u và số cách là v thì <br>
Số tờ ít nhất cần lấy ra để có tổng bằng W là **&sum;<sup>c</sup><sub>i = 0</sub>(u)** <br>
Số cách ứng với cách lấy trên là **&prod;<sup>c</sup><sub>i = 0</sub>(v)**


Với TH **(2) và (3)** ( z,t &isin; [0, 9]) ta có thể lập bảng để tra cứu như sau

||Số tờ | Số cách |
| -------- | -------- | -------- |
| 0     | 0     | 1     |
| 1     | 1     | 1     |
| 2     | 1     | 1     |
| 3     | 1     | 1     |
| 4     | 2     | 2 (dùng tờ số 3 và 1 hoặc 2 tờ số 2)     |
| 5     | 1     | 1     |
| 6     | 2     | 2 (dùng tờ số 5 và 1 hoặc 2 tờ số 3)    |
| 7     | 2     | 1     |
| 8     | 2     | 1     |
| 9     | 3     | 3 (dùng tờ số 5, 3 và 1 hoặc 5 và 2 tờ số 2 hoặc 3 tờ số 3)    |


<br>

Với TH **(1)** ta tiếp tục tham lam bằng cách sử dụng tờ mệnh giá cao nhất tờ số 5 để lấy ra **xy/5** đồng phần dư còn lại xy%5 ta tiếp tục tra bảng trên. <br>
Ví dụ xy = 29 <br>
Ta sử dụng 5 tờ 5 đồng để lấy được 25 đồng ( chỉ có 1 cách) sau đó còn 4 đồng ta dùng 2 tờ ( có 2 cách) 
Tuy nhiên ta còn 1 cách lấy nữa đó là chỉ lấy 4 tờ 5 đồng để được 20 đồng và dùng 3 tờ 3 đồng 
=> với TH này ta cần 7 tờ và có tới tận 3 cách lấy

Tổng quát lại ta có các TH sau<br>
1. xy%5 = 0 => cần xy/5 tờ 5 đồng và có 1 cách lấy
2. xy%5 = 1 => có 2 TH con 
* 1.  xy != 1 => có thêm cách là lấy xy/5 -1 tờ 5 đồng và lấy 6 đồng nữa => cần xy/5 + 1 tờ và có 2 cách lấy
* 2. xy đúng bằng 1 => cần 1 tờ 1 đồng và có 1 cách lấy
3. xy%5 = 2 => có 2 TH con tuy nhiên với TH lấy 7 đồng cũng chỉ có 1 cách => cần xy/5 + 1 và có 1 cách
4. xy%5 = 3 => có 2 TH con tuy nhiên với TH lấy 8 đồng cũng chỉ có 1 cách => cần xy/5 + 1 và có 1 cách
5. xy%5 = 4 => có 2 TH con
* 1. xy != 4 => có thêm cách là lấy xy/5 -1 tờ 5 đồng và lấy 6 đồng nữa => cần xy/5 + 1 tờ và có 3 cách lấy
* 2.  xy == 4 => cần 4 đồng => cần 2 tờ và có 2 cách lấy

**Implementation**

```C

void proc(long long w, int c)
{
    int so_cach[]= {1,1,1,1,2,1,2,1,1,3};
    int so_to[]= {0,1,1,1,2,1,2,2,2,3};
    
    long long cach=1,to=0;
    int d;
    while (c > 0 && w > 0)
    {
        c--;
        d = w%10;
        w /= 10;
        to += so_to[d];
        cach *= so_cach[d];
    }

    if (w == 0)
    {
        cout << to << " " << cach << endl;
        return;
    }
    if (w%5 == 0)
    {
        to += w/5;
    }
    else if (w%5 == 1)
    {
        to += w/5 + 1;
        if (w != 1)
            cach *= 2;
    }
    else if (w%5 == 2)
    {
        to += w/5 +1;
    }
    else if (w%5 == 3)
    {
        to += w/5 +1;
    }
    else if (w%5 == 4)
    {
        to += w/5 +2;
        if (w != 4)
            cach*= 3;
        else
            cach*= 2;
    }
    cout << to << " " << cach << endl;
}
```

## Bài toán cái túi
**Problem:** Một kẻ trộm đột nhập vào một cửa hiệu tìm thấy có n mặt hàng có trọng lượng và giá trị khác nhau, nhưng hắn chỉ mang theo một cái túi có sức chứa về trọng lượng tối đa là M. Vậy kẻ trộm nên bỏ vào ba lô những món nào và số lượng bao nhiêu để đạt giá trị cao nhất trong khả năng mà hắn có thể mang đi được.<br>
Thực chất đây là 1 bài toán tối ưu hóa tổ hợp. Bài toán cơ bản nhất là bài toán cái túi dạng 0-1 tức kẻ trộm sẽ chọn (chọn 1 đồ vật) và không chọn. <br>
        ![](https://images.viblo.asia/dabbeb0a-0531-4768-b605-5fc054cbfdd6.jpg)

Bài toán cái túi có cách tiếp cận tối ưu là quy hoạch động. Tuy nhiên không phải bài toán NP-hard nào cũng có thể giải bằng quy hoạch động được và ban đầu bài toán cái túi cũng được tiếp cận qua tham lam.<br>
Link tham khảo: https://en.wikipedia.org/wiki/Continuous_knapsack_problem <br>
**Idea:** Ý tưởng tham lam của bài toán đó là tính tỷ lệ giá trị/khối lượng của từng vật  p<sub>i</sub>/w<sub>i</sub>  (price/weight). Sau đó sort theo tỷ lệ này với thứ tự giảm dần. Chúng ta sẽ chọn những vật có p<sub>i</sub>/w<sub>i</sub> cao nhất đồng thời sức chứa của túi có thể chứa được vật đó (remain >  w<sub>i</sub>) mỗi khi cho thêm vật vào túi ta cũng giảm sức chứa của túi đi.<br>
**Implementation:**
```C
    int p[]; // gia tri cua vat thu i
	int w[]; // khoi luong cua vat thu i

	void swap(int i, int j)
    {
        int tmp = p[i];
        p[i] = p[j];
        p[j] = tmp;
        tmp = w[i];
        w[i] = w[j];
        w[j] = tmp;
    }

    void bubblesort(int a[], int b[], int N)
    {
        for (int i = 0; i < N-1; i++)
        for (int j = i+1; j < N; j++)
        	if ((double) p[i]/w[i] < (double) p[j]/w[j]) 
        		swap(i, j);
    }


    int proc(int W, int N) 
    { 
        bubble_sort(p, w, N); // sort theo ty le p[i]/w[i]

        int remain = W;  
        int res = 0.0; 

        for (int i = 0; i < n; i++) 
        { 
            if (w[i] <= remain) 
            { 
                remain -= w[i]; 
                res += p[i]; 
            }
        } 
        return res; 
    } 
```

# Các bài toán liên quan tới đồ thị
Đa số các bài toán tham lam đều trong đồ thị dưới đây là một số ví dụ điển hình
## Bài toán cây khung nhỏ nhất
**Problem:** Cho 1 đồ thị vô hướng và liên thông có trọng số G = (V, E) cây khung của đồ thị G là một cây bao trùm lên đồ thị G, cây khung nhỏ nhất của một đồ thị liên thông có trọng số là cây khung có tổng trọng số các cạnh là nhỏ nhất. 
<br>
**Idea:** Một trong những thuật toán tìm cây khung nhỏ nhất là Kruskal (đây là 1 thuật toán tham lam). Ý tưởng của nó như sau<br>
Thuật toán Kruskal dựa trên mô hình xây dựng cây khung nhỏ nhất bằng thuật toán hợp nhất.
*  Thuật toán không xét các cạnh với thứ tự tuỳ ý.
*  Thuật toán xét các cạnh theo thứ tự đã sắp xếp theo trọng số.<br>

Để xây dựng tập n-1 cạnh của cây khung nhỏ nhất - tạm gọi là tập K, Kruskal đề nghị cách kết nạp lần lượt các cạnh vào tập đó theo nguyên tắc như sau:
* Ưu tiên các cạnh có trọng số nhỏ hơn.
* Kết nạp cạnh khi nó không tạo chu trình với tập cạnh đã kết nạp trước đó.
* Đó là một nguyên tắc chính xác và đúng đắn, đảm bảo tập K nếu thu đủ n - 1 cạnh sẽ là cây khung nhỏ nhất.<br>

Như vậy thuật toán tham lam bằng cách luôn chọn những cạnh có trọng số nhỏ nhất và không tạo thành chu trình để kết nạp. Để xác định cạnh được kết nạp có tạo chu trình hay không ta có thể sử dụng cấu trúc dữ liệu **disjoin set**

**Implementation**
```C
// data structure for disjoint-set
int rank[MAX];// rank[v] la rank cua set v
int p[MAX];// p[v] la cha cua v

void link(int x, int y){
    if(rank[x] > rank[y]) p[y] = x;
    else{
        p[x] = y;
        if(rank[x] == rank[y]) rank[y] = rank[y] + 1;
    }
}
void makeSet(int x){
    p[x] = x;
    rank[x] = 0;
}
int findSet(int x){
    if(x != p[x])
        p[x] = findSet(p[x]);
    return p[x];
}

void swap(int&a, int&b){
    int tmp = a; a = b; b = tmp;
}
void swapEdge(int i, int j){
    swap(c[i],c[j]);
    swap(u[i],u[j]);
    swap(v[i],v[j]);
}
int partition(int L, int R, int index){
    int pivot = c[index];
    swapEdge(index,R);
    int storeIndex = L;
    for(int i = L; i <= R-1; i++){
        if(c[i] < pivot){
            swapEdge(storeIndex,i);
            storeIndex++;
        }
    }
    swapEdge(storeIndex,R);
    return storeIndex;
}
void quickSort(int L, int R){
    if(L < R){
        int index = (L+R)/2;
        index = partition(L,R,index);
        if(L < index) quickSort(L,index-1);
        if(index < R) quickSort(index+1,R);
    }
}

void solve(){
    for(int x = 1; x <= N; x++) {
        makeSet(x);
    }
    quickSort(0,M-1); // sort các cạnh theo trọng số
    int res = 0;
    int count = 0;
    for(int i = 0;i < M; i++){
        // xem xet canh (u[i],v[i])
        int ru = findSet(u[i]);
        int rv = findSet(v[i]);
        if(ru != rv) { // rank của u khac rank cua v tức u, v chưa có liên kết -> thêm cạnh uv vào không tạo ra chu trình
            link(ru,rv);
            res += c[i];
            count++;
            if(count == N-1) break;
        }
    }
    cout << res; // trọng số của cây khung nhỏ nhất 
}
```

## Bài toán  tìm đường đi ngắn nhất
**Problem:** Cho đồ thị G có trọng số không âm, tìm một đường đi giữa hai đỉnh sao cho tổng các trọng số của các cạnh tạo nên đường đi đó là nhỏ nhất. 

**Idea:**  Một trong những thuật toán tìm cây khung nhỏ nhất là Dijsktra (đây là 1 thuật toán tham lam). Ý tưởng của nó như sau:
* Đặt tất cả các khoảng cách đỉnh = vô cùng ngoại trừ đỉnh xuất phát = 0.
* Đẩy đỉnh xuất phát trong hàng đợi ưu tiên.
* Pop đỉnh với khoảng cách từ đỉnh đó tới đỉnh xuất phát là nhỏ nhất (nếu xây dựng hàng đợi ưu tiên sử dụng min-heap thì đó chính là đỉnh đầu tiên).
* Sử dụng đỉnh được lấy ra cập nhật khoảng cách của các đỉnh kề với nó tới đỉnh xuất phát trong trường hợp "khoảng cách đỉnh hiện tại + trọng lượng cạnh < khoảng cách đỉnh kề với nó tới đỉnh xuất phát", sau đó đẩy các đỉnh kề vào hàng đợi.
* Lặp lại cho đến khi hàng đợi ưu tiên trống.

**Implementation**
```C

    struct Edge{
    int x;
    int w;
    };

    int N, M;
    int NN;
    set<Edge*> A[MAX];// A[v] la tap cac canh (x,w) ke voi v: canh (v,x) co trong so w
    set<int> S;// tap ung cu vien, ket nap dan vao cay khung (loi giai)
    int d[MAX];
    int pred[MAX];
    bool found[MAX];
    int s,t; // đỉnh bắt đầu và xuất phát
    int node[MAX];
    int idx[MAX];
    
    void input() {
        // đọc đồ thị
        cin >> N >> M;NN = N;
        for(int i = 0; i < M; i++){
            int u,v,w;
            cin >> u >> v >> w;// canh (u,v) co trong so w
            Edge* e = new Edge;
            e->x = v;
            e->w = w;
            A[u].insert(e);

            e = new Edge;
            e->x = u;
            e->w = w;
            A[v].insert(e);
        }
        cin >> s >> t;
    }

    void swap(int i, int j){
        int tmp = d[i]; d[i] = d[j]; d[j] = tmp;

        tmp = node[i]; node[i] = node[j]; node[j] = tmp;
        idx[node[i]] = i; idx[node[j]] = j;
    }
    
    void heapify(int i, int n){
        int L = 2*i;
        int R = 2*i+1;
        int min = i;
        if(L <= n && d[L] < d[i]) min = L;
        if(R <= n && d[R] < d[min]) min = R;
        if(min != i){
            swap(i,min);
            heapify(min,n);
        }
    }
    
    void buildHeap(){
        for(int i = N/2; i >= 1; i--)
            heapify(i,N);
    }
    
    void init(){
        for(int i = 1; i <= N; i++){
            node[i] = i;
            idx[i] = i;
        }
        for(int v = 1; v <= N; v++)
            d[v] = 100000000;
        d[idx[s]] = 0;
        
        for(set<Edge*>::iterator q = A[s].begin(); q != A[s].end(); q++){
            Edge* e = *q;
            int v = e->x;
            int w = e->w;
            d[idx[v]] = w;
            pred[v] = s;
        }
        buildHeap();
    }
    
    void upHeap(int i){
        int p = i;
        while(p/2 >= 1){
            if(d[p] < d[p/2]){
                swap(p,p/2);
                p = p/2;
            }else
                break;
        }
    }
    
    int selectMin(){
        int sel_v = node[1];
        swap(1,N);
        N = N-1;
        heapify(1,N);
        return sel_v;
    }

    void solve(){
        for(int v = 1; v <= N; v++)if(v != s)
            found[v] = false;
        found[s] = true;
        init();
        int s = selectMin();
        

        while(N > 0){
            int v = selectMin();
            found[v] = true;
            for(set<Edge*>::iterator q = A[v].begin(); q != A[v].end(); q++){
                Edge* e = *q;
                int u = e->x;
                if(found[u] == false){ // kiem tra u co thuoc S hay khong
                    int w = e->w;
                    if(d[idx[u]] > d[idx[v]] + w){
                        d[idx[u]] = d[idx[v]] + w;
                        pred[u] = v;
                        upHeap(idx[u]);
                    }
                }
            }
        }
        cout << "result = " << d[idx[t]] << endl;

        return;
        stack<int> path;
        int x = t;
        while(x != s){
            path.push(x);
            x = pred[x];
        }
        cout << s;
        while(!path.empty()){
            int x = path.top(); path.pop();
            cout << " --> " << x;
        }
    }
```