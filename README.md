# Lý thuyết trò chơi và Định lý Sprague - Grundy
#### Tác giả: 
- Đỗ Phúc An Nguyên
- Phạm Nguyễn Đăng Huy


## Bài toán mở đầu
Cho 1 đồ thị không chu trình có hướng, không trọng số, gồm $n$ đỉnh và $m$ cạnh (bạn có thể xem lại các khái niệm cơ bản về đồ thị tại [đây.](https://vi.wikipedia.org/wiki/Cây_(lý_thuyết_đồ_thị))

Bá Phúc và Đức Mạnh quyết định chơi một trò chơi trên đồ thị này (sau khi bị thu máy). Cụ thể, trò chơi như sau: 
- Đầu tiên, một đồng xu được đặt trên 1 đỉnh bất kì của đồ thị. 
- Tiếp theo, trò chơi diễn ra luân phiên từng người một: Bá Phúc đi trước, Đức Mạnh đi sau.
-  Đến lượt mình, mỗi người chơi chọn một đỉnh mà đỉnh chứa đồng xu ở lượt này có cạnh nối tới, sau đó, dịch đồng xu tới đỉnh đó. Nói cách khác, người chơi có thể di chuyển đồng xu từ đỉnh $u$ tới đỉnh $v$ khi và chỉ khi $u$ có cạnh nối tới $v$ ($u  \rightarrow v$).
- Người đầu tiên không thể di chuyển đồng xu ở lượt cùa mình sẽ thua cuộc.

Đức Mạnh, với kinh nghiệm hơn $1000$ trận mỗi mùa giải, đã nhanh chóng nhìn ra chiến thuật chơi tối ưu, tuy nhiên, Bá Phúc vắt óc mãi mà không biết chơi như thế nào. Bạn hãy giúp Bá Phúc đếm số đỉnh, mà nếu chơi tối ưu, có thể thắng được Đức Mạnh.
$\downarrow$


## Khái niệm
Lý thuyết trò chơi (Game Theory) là một dạng lý thuyết toán học nâng cao, được sử dụng trong xác suất, thống kê (và bet). Có 3 dạng lý thuyết trò chơi chính, bao gồm toán bất biến, quy hoạch động trò chơi (Game DP), và Nim. Tuy nhiên, tất cả các dạng trò chơi đều chỉ tuân theo một quy luật duy nhất (ít nhất trong lập trình thì là vậy). Vậy đó là quy luật gì?

## Quy luật chung
Các trò chơi đơn giản (phân định thắng thua, nhiều lượt, thường là 2 người chơi và chỉ có 1 đối tượng, không có yếu tổ may rủi) luôn tuân theo một số nguyên lí cố định.
1. Luôn có một số trạng thái mà người chơi nào ở trạng thái đó sẽ thua cuộc, gọi là trạng thái thua (losing state). Trạng thái mà khiến người chơi thua ngay tức thì (ví dụ, > 21 điểm trong xì dách, cháy trong phỏm) gọi là trạng thái thua cơ bản (base losing state), còn trạng thái mà nếu chơi tối ưu chắc chắn sẽ thua (ví dụ, cùng team Đức Mạnh) gọi là trạng thái thua tiềm năng (potential losing state).
2. Các trạng thái còn lại là trạng thái thắng (winning state). 
3. Tất cả các trạng thái đều đi về trạng thái thua cơ bản.
4. Trong một lượt chơi trạng thái thắng có thể đi tới cả trạng thái thắng và thua; trạng thái thua chỉ có thể đi tới trạng thái thắng.

## Trở lại bài toán mở đầu
#### Nhận xét:
Vì đồ thị không có chu trình nên trò chơi sẽ kết thúc sau tối đa $n - 1$ bước. Nhận thấy được rằng các đỉnh lá (không có cạnh nối tới đỉnh nào khác) là đỉnh chứa các trạng thái thua (đỉnh thua). Vậy các đỉnh có cạnh nối trực tiếp tới đỉnh lá (đỉnh bậc 1) là đỉnh thắng. Tương tự, các đỉnh chỉ có cạnh nối tới đỉnh bậc 1 là đỉnh thua, và cứ thế...

#### Ví dụ:
Xét đồ thị sau:

![image](https://github.com/user-attachments/assets/ce8603ee-b779-438d-be22-da4f1cc97f2d)

Có thể thấy $1, 3$ là đỉnh thua do là đỉnh lá, $2, 4$ là đỉnh thắng do có cạnh nối trực tiếp tới lá, ... Như vậy, ta có các đỉnh thua là $1, 3, 6$; các đỉnh thắng là $2, 4, 5$.


#### Cách giải:
Gọi $f(u)$ là trạng thái của đỉnh $u$. $f(u) = 1$ là đỉnh $u$ thắng, $f(u) = 0$ là đỉnh $u$ thua. Ta tính hàm $f(u)$ như sau:
- $f(leaf) = 0$ với mọi đỉnh $leaf$ lá.
- $f(u) = [f(v_1) \land f(v_2) \land ... \land f(v_k)] \oplus 1$ với mọi $(u \rightarrow v_1), (u \rightarrow v_2), ..., (u \rightarrow v_k)$.  Nói cách khác, $f(u) = 0$ khi và chỉ khi mọi $v$ sao cho $(u \rightarrow v)$ thỏa mãn $f(v) = 1$.

Vậy, ta có cách giải như sau:
- Khởi tạo mảng $f[u]$, ban đầu có giá trị $-1$. 
- Tìm các đỉnh $leaf$ lá và đặt $f[leaf] = 0$.
- Với mỗi đỉnh $u$ còn lại, ta thực hiện DFS tới các đỉnh xung quanh và tính $f[u]$ bằng công thức trên.
- Kết quả là số đỉnh $u$ thỏa mãn $f[u] = 1$.
<details>
<summary>Code mẫu</summary>
	
```cpp
//author: toberu
#include<bits/stdc++.h>
#define int long long
#define vi vector <int>
#define fastio ios_base::sync_with_stdio(0); cin.tie(0);
using namespace std;
const int N = 1e6 + 5;
int n, m, f[N], deg[N];
vi g[N];
void dfs(int u){
	f[u] = 1;
	for(int v : g[u]){
		if(f[v] == -1)dfs(v);
		f[u] &= f[v];
	}
	f[u] ^= 1;
}
signed main(){
	fastio;
	cin >> n >> m;
	for(int i = 1; i <= m; i++){
		int u, v; cin >> u >> v;
		g[u].push_back(v); deg[u]++;
	}
	fill(f + 1, f + 1 + n, -1);
	for(int i = 1; i <= n; i++)if(deg[i] == 0)f[i] = 0;
	for(int i = 1; i <= n; i++)if(f[i] == -1)dfs(i);
	cout << accumulate(f + 1, f + 1 + n, 0);
}
```

</details>
		
*Đây là một bài thuộc loại quy hoạch động trò chơi điển hình.*

## Lý thuyết trò chơi: Toán bất biến và trò chơi Nim
Cả 2 dạng đều có chung đặc điểm: mọi trạng thái thua của một trò chơi đều có chung một tính chất toán học. Thực tế, Nim là lĩnh vực nâng cao và chuyên biệt hơn của toán bất biến, nên chúng ta sẽ bắt đầu với toán bất biến trước.

### A. Toán bất biến

#### Bài toán mở đầu
Ngọc Minh và Thuận Hiếu, quê Hải Ngôn, là một đôi bạn thân. Một hôm, đang đi daọ, họ "tình cờ" tìm được 1 cái ví, có $n$ đồng xu giống hệt nhau. 2 người quyết định chơi 1 trò chơi như sau:
- Trò chơi diễn ra luân phiên theo luật, bắt đầu từ Ngọc Minh.
- Mỗi lượt, người chơi sẽ bốc một nắm xu ra. Vì luyện tập cơ ngón tay, nên cả 2 đều có thể bốc lên tới $k$ viên sỏi.
- Ai bốc hết xu được trước là người thắng cuộc.

Hỏi: cho trước $n$ và $k$, liệu có thể biết trước người thắng cuộc?

**Một cách làm:** sử dụng quy hoạch động trò chơi để tính $f(n)$ ($1$ nếu Ngọc Minh thắng và $0$ nếu ngược lại). Công thức tổng quát như sau:
- $f(0) = 0$
- $f(n) = [f(n - 1) \land f(n - 2) \land ... f(n - min(n, k))] \oplus 1$
<details>
<summary>Code mẫu</summary>
	
```cpp
cin >> n >> k;
int f[n + 1];
f[0] = 0;
for(int i = 1; i <= n; i++){
	f[i] = 1;
	for(int j = 1; j <= i, j <= k; j++)f[i] &= f[j];
	f[i] ^= 1;
}
```
 
</details>

Cách làm này có độ phức tạp là $O(nk)$, nhưng ta có thể tối ưu xuống $O(n)$ bằng cách tính trước $x = [f(n - 1) \land f(n - 2) \land ... f(n - min(n, k))]$. Nhận thấy $x = 1 \Longleftrightarrow$ $$\sum_{m=max(0,n-k)}^{n-1} f(m)$$ $= min(n - 1, k)$. Vậy ta có thể sử dụng biến $cur$ để kiểm soát tổng cộng dồn và tính cho $f(n)$.
<details>
<summary>Code mẫu</summary>
	
```cpp
cin >> n >> k;
int f[n + 1], sum = 0;
f[0] = 0;
for(int i = 1; i <= n; i++){
	int ok = 0;
	if(sum == min(i - 1, k))ok = 1;
	f[i] = ok^1;
	sum += f[i];
	if(i >= k)sum -= f[i - k];
}
```
 
</details>

Tuy nhiên, ta vẫn có thể cải tiến hơn nữa.
Từ nhận xét $f(0) = 0$, $f(1) = f(2) = ... = f(k) = 1$, $f(k + 1) = 0$, $f(k + 2) = f(k + 3) = ... = f(2k + 1) = 1$, ... Có thể nhận thấy $f(n) = 0$ khi và chỉ khi $n$ chia hết cho $(k + 1)$. Vậy ta có thể xác định người thắng trong $O(1)$.

#### Nhận xét
Với những bài toán về lý thuyết trò chơi liên quan đến số học, hãy tìm một trạng thái chung mà:
- Có chung tính chất với trạng thái thua cơ bản, và
- Chỉ có thể trực tiếp đi đến các trạng thái còn lại (Không thể trực tiếp đi đến trạng thái khác cùng tính chất đó).

Trong bài toán ví dụ trên, trạng thái chung là $n_t$ $mod$ $(k + 1) = 0$, vì không tồn tại $n_x$, $n_y$ thỏa mãn $|n_x - n_y| \leq k$, hay $n_x \rightarrow n_y$ hoặc $n_x \leftarrow n_y$.
