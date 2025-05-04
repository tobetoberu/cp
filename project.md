# Lý thuyết trò chơi và Định lý Sprague - Grundy
#### Tác giả: 
- Đỗ Phúc An Nguyên
- Phạm Nguyễn Đăng Huy


## Bài toán mở đầu
Cho 1 đồ thị không chu trình có hướng, không trọng số, gồm $n$ đỉnh và $m$ cạnh (bạn có thể xem lại các khái niệm cơ bản về đồ thị tại [đây.](https://vi.wikipedia.org/wiki/Cây_(lý_thuyết_đồ_thị))

Bá Phúc và Đức Mạnh quyết định chơi một trò chơi trên đồ thị này (sau khi bị thu máy). Cụ thể, trò chơi như sau: 
- Đầu tiên, một đồng xu được đặt trên 1 đỉnh bất kì của đồ thị. 
- Tiếp theo, trò chơi diễn ra luân phiên từng người một: Phúc đi trước, Đức Mạnh đi sau.
-  Đến lượt mình, mỗi người chơi chọn một đỉnh mà đỉnh chứa đồng xu ở lượt này có cạnh nối tới, sau đó, dịch đồng xu tới đỉnh đó. Nói cách khác, người chơi có thể di chuyển đồng xu từ đỉnh $u$ tới đỉnh $v$ khi và chỉ khi $u$ có cạnh nối tới $v$ ($u  \rightarrow v$).
- Người đầu tiên không thể di chuyển đồng xu ở lượt cùa mình sẽ thua cuộc.

Đức Mạnh, với kinh nghiệm hơn $1000$ trận mỗi mùa giải, đã nhanh chóng nhìn ra chiến thuật chơi tối ưu, tuy nhiên, Bá Phúc vắt óc mãi mà không biết chơi như thế nào. Bạn hãy giúp Bá Phúc đếm số đỉnh, mà nếu chơi tối ưu, có thể thắng được Đức Mạnh.
$\downarrow$


## Khái niệm
Lý thuyết trò chơi (Game Theory) là một dạng lý thuyết toán học nâng cao, được sử dụng trong xác suất, thống kê (và bet). Có 3 dạng lý thuyết trò chơi chính, bao gồm toán bất biến, quy hoạch động trò chơi (Game DP), và Nim. Tuy nhiên, tất cả các dạng trò chơi đều chỉ tuân theo một quy luật duy nhất (ít nhất trong lập trình thì là vậy). Vậy đó là quy luật gì?

## Quy luật chung
Các trò chơi đơn giản (phân định thắng thua, nhiều lượt, chỉ có 1 đối tượng, 1 mục tiêu, không có yếu tổ may rủi) luôn tuân theo một số nguyên lí cố định.
1. Luôn có một số trạng thái mà người chơi nào ở trạng thái đó sẽ thua cuộc, gọi là trạng thái thua (losing state). Trạng thái mà khiến người chơi thua ngay tức thì (ví dụ, > 21 điểm trong xì dách, cháy trong phỏm) gọi là trạng thái thua cơ bản (base losing state), còn trạng thái mà nếu chơi tối ưu chắc chắn sẽ thua (ví dụ, cùng team Đức Mạnh) gọi là trạng thái thua tiềm năng (potential losing state).
2. Các trạng thái còn lại là trạng thái thắng (winning state). 
3. Tất cả các trạng thái đều đi về trạng thái thua cơ bản.
4. Trong một lượt chơi trạng thái thắng có thể đi tới cả trạng thái thắng và thua; trạng thái thua chỉ có thể đi tới trạng thái thắng.

## Trở lại bài toán mở đầu
#### Nhận xét:
Vì đồ thị không có chu trình nên trò chơi sẽ kết thúc sau tối đa $n - 1$ bước. Nhận thấy được rằng các đỉnh lá (không có cạnh nối tới đỉnh nào khác) là đỉnh chứa các trạng thái thua (đỉnh thua). Vậy các đỉnh có cạnh nối trực tiếp tới đỉnh lá (đỉnh bậc 1) là đỉnh thắng. Tương tự, các đỉnh chỉ có cạnh nối tới đỉnh bậc 1 là đỉnh thua, và cứ thế...

#### Ví dụ:
Xét đồ thị sau:

![image](https://i.imgur.com/bWlke5F.png)

Có thể thấy $1, 3$ là đỉnh thua do là đỉnh lá, $2, 4$ là đỉnh thắng do có cạnh nối trực tiếp tới lá, ... Như vậy, ta có các đỉnh thua là $1, 3, 6$; các đỉnh thắng là $2, 4, 5$.


#### Cách giải:
Gọi $f(u)$ là trạng thái của đỉnh $u$. $f(u) = 1$ là đỉnh $u$ thắng, $f(u) = $0$$ là đỉnh $u$ thua. Ta tính hàm $f(u)$ như sau:
- $f(leaf) = $0$$ với mọi đỉnh $leaf$ lá.
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
	for(int i = 1; i <= n; i++)if(deg[i] == 0)f[i] = $0$;
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
Minh và Hiếu, quê Hải Ngôn, là một đôi bạn thân. Một hôm, đang đi daọ, họ "tình cờ" tìm được 1 cái ví, có $n$ đồng xu giống hệt nhau. 2 người quyết định chơi 1 trò chơi như sau:
- Trò chơi diễn ra luân phiên theo luật, bắt đầu từ Ngọc Minh.
- Mỗi lượt, người chơi sẽ bốc một nắm xu ra. Vì luyện tập cơ ngón tay, nên cả 2 đều có thể bốc lên tới $k$ viên sỏi.
- Ai bốc hết xu được trước là người thắng cuộc.

Hỏi: cho trước $n$ và $k$, liệu có thể biết trước người thắng cuộc?

**Một cách làm:** sử dụng quy hoạch động trò chơi để tính $f(n)$ ($1$ nếu Minh thắng và $0$ nếu ngược lại). Công thức tổng quát như sau:
- $f(0) = 0$
- $f(n) = [f(n - 1) \land f(n - 2) \land ... f(n - min(n, k))] \oplus 1$
<details>
<summary>Code mẫu</summary>
	
```cpp
cin >> n >> k;
int f[n + 1];
f[$0$] = $0$;
for(int i = 1; i <= n; i++){
	f[i] = 1;
	for(int j = 1; j <= i, j <= k; j++)f[i] &= f[j];
	f[i] ^= 1;
}
```
 
</details>

Cách làm này có độ phức tạp là $O(nk)$, nhưng ta có thể tối ưu xuống $O(n)$ bằng cách tính trước $x = [f(n - 1) \land f(n - 2) \land ... f(n - min(n, k))]$. Nhận thấy $x = 1$ $\Longleftrightarrow$ \sum_{m=max(0,n-k)}^{n-1} f(m)$ $= min(n - 1, k)$. Vậy ta có thể sử dụng biến $cur$ để kiểm soát tổng cộng dồn và tính cho $f(n)$.
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

Trong bài toán ví dụ trên, trạng thái chung là $n_t$ $mod$ $(k + 1) = 0$, vì không tồn tại $n_x, n_y$ thỏa mãn $n_x \rightarrow n_y$ hoặc $n_x \leftarrow n_y$ ($|n_x - n_y| \leq k$) với $x \neq y$.

### B. Bài toán Nim

#### Phát biểu bài toán
Cho $n$ đống sỏi, ban đầu mỗi đống sỏi có $A_i$ viên sỏi ($1 \leq i \leq n$). Hai người chơi bốc sỏi theo lượt, mỗi lượt một người có thể bốc $k$ viên sỏi từ đống $i$ ($1 \leq k \leq A_i$) nếu $(k, A_i)$ thỏa mãn điều kiện $C$. Trong bài toán Nim cơ bản, không xét điều kiện $C$, tức là có thể bốc bất kỳ số viên sỏi nào.

#### Giá trị Nim
Mục tiêu là xác định ai sẽ thắng nếu cả hai người chơi đều tối ưu, hay nói cách khác là tìm chiến thuật thắng.

Khi bắt đầu giải, ta sẽ gặp phải vấn đề là trạng thái của trò chơi được biểu diễn bằng bộ các số nguyên, thay vì một số nguyên duy nhất. Do đó, cần phải tìm cách chuyển trạng thái của trò chơi thành một giá trị duy nhất.

#### Trường hợp đơn giản: Một đống sỏi
Khi chỉ có một đống sỏi, giá trị Nim là số viên sỏi còn lại trong đống. Nếu $p = 0$, trạng thái này thuộc về tập thua, ngược lại nếu $p > 0$, trạng thái này thuộc về tập thắng. Điều này phải đúng với trường hợp tổng quát.

Vậy phép toán $\oplus$ phải có 3 tính chất
- **Kết hợp và giao hoán**: Tức là thứ tự của các đống sỏi không ảnh hưởng đến kết quả cuối cùng.
- **Phần tử trung hòa**: Phép XOR với $0$ không thay đổi giá trị.
- **Phần tử đối**: $a \oplus a = 0$, vì sao? Giả sử có $2$ đống sỏi giá trị như nhau, người chơi sau có thể sao chép nước đi của người chơi trước ở đống sỏi còn lại, điều này đảm bảo chiến thắng cho người chơi sau.

Nếu chỉ xét 2 tính chất đầu, ta có thể nghĩ tới phép cộng, tuy nhiên, với tính chất phần tử đối, ta phải nghĩ tới một phép toán khác liên quan đến triệt tiêu
> Phép XOR thực chất là phép cộng modulo 2 trên từng bit.

Ví dụ, với 3 đống sỏi có số viên lần lượt là ${1, 4, 5}$, tổng Nim là:

$1 \oplus 4 \oplus 5 = 0$


Kết quả của phép toán XOR trên các đống sỏi sẽ quyết định ai thắng:

- Nếu tổng Nim $= 0$, người chơi thứ hai thắng.
- Nếu tổng Nim $\neq 0$, người chơi thứ nhất thắng.

#### Cách xác định người thắng trong trò chơi Nim
Để xác định người thắng, ta chỉ cần tính tổng Nim của tất cả các đống sỏi. Nếu tổng Nim khác $0$, người đi trước có chiến thuật thắng. Ngược lại, nếu tổng Nim bằng $0$, người đi sau sẽ thắng, vì họ có thể phản lại mọi nước đi của người đi trước.

#### Code cài đặt Nim
```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
    int n;
    cin >> n;
    int result = 0;
    for (int i = 0; i < n; i++) {
        int a;
        cin >> a;
        result ^= a;
    }
    if (result == 0)
        cout << "B wins\n";  // Người đi sau thắng
    else
        cout << "A wins\n";  // Người đi trước thắng
    return 0;
}
```

### C. Misere Nim

Trong trò chơi **Misere Nim**, các quy tắc giống như trò chơi **Nim** bình thường, nhưng người chơi lấy viên sỏi cuối cùng sẽ thua, hoặc người chơi không thể di chuyển (vì không còn viên sỏi) sẽ thắng.

#### Giải pháp cho trò chơi Nim bình thường

Trong trò chơi **Nim bình thường**, nếu tổng XOR của tất cả các đống bằng $0$, người chơi hiện tại sẽ thua. Nếu không, người chơi tiếp theo sẽ thua.

#### Trường hợp 1: Tổng XOR ban đầu bằng $0$

Nếu tổng XOR ban đầu là $0$, người chơi 1 không thể tránh khỏi việc tạo ra tổng Nim không phải $0$, khiến người chơi 2 vào vị trí thắng. Cuối cùng, người chơi 1 sẽ thua.

#### Trường hợp 2: Tổng XOR ban đầu không bằng $0$

Nếu tổng XOR ban đầu không phải $0$, người chơi 1 sẽ đưa tổng Nim về 0, ép người chơi 2 vào vị trí thua. Người chơi 1 sẽ thắng.

#### Giải pháp cho trò chơi Misere Nim

Trong trò chơi Misere Nim, tổng Nim bằng $0$ có vẻ như là một vị trí thắng. Tuy nhiên, vấn đề là ai sẽ lấy viên sỏi cuối cùng. Nếu người chơi 1 có thể ép người chơi 2 lấy viên sỏi cuối cùng, người chơi 1 sẽ thắng.

Nếu trạng thái tổng Nim là $0$ và có hơn một đống chứa viên sỏi, người chơi 1 sẽ ép người chơi 2 vào vị trí không thể thắng. Tuy nhiên, nếu tất cả các đống chỉ chứa một viên sỏi, có một trường hợp đặc biệt cần xử lý.

##### Trong trường hợp đặc biệt

- Nếu số lượng đống là chẵn, người chơi 1 thắng.
- Nếu số lượng đống là lẻ, người chơi 2 thắng.

#### Kết luận

- Trong trò chơi **Nim bình thường**, tổng XOR bằng $0$ là trạng thái thua.
- Trong trò chơi **Misere Nim**, tổng XOR bằng $0$ vẫn là thua, nhưng cần xử lý các trường hợp đặc biệt với các đống chỉ có một viên sỏi.

#### Kết luận:
Sử dụng Grundy Number giúp xác định chiến lược tối ưu trong các trò chơi Nim, giúp người chơi quyết định khi nào nên đi và những nước đi nào là tối ưu.

### D. Định lý Bouton
    
Gọi $\mathcal{P}$ là tập chứa trạng thái thua (tổng XOR $s = 0$), $\mathcal{N}$ là các tập còn lại. Ta chứng minh giá trị XOR $s$ thỏa mãn các nguyên lí cơ bản.
#### Nguyên lí 1: Trạng thái kết thúc ∈ $\mathcal{P}$
**Chứng minh**:  
Trạng thái $(0,0,...,0)$ có:
$$0 \oplus 0 \oplus ... \oplus 0 = 0$$
⇒ Thuộc $\mathcal{P}$.

#### Nguyên lí 2: Từ $\mathcal{N}$ có thể đến $\mathcal{P}$
**Chứng minh**:  
Giả sử $s = x_1 \oplus ... \oplus x_n > 0$. Chọn đống $x_k$ có bit cao nhất của $s$ bật (luôn tồn tại vì $s≠0$).

Đặt $x_k' = x_k \oplus s$. Vì bit cao nhất bị đảo nên $x_k' < x_k$.

Trạng thái mới có tổng Nim:
$$
\begin{aligned}
&x_1 \oplus ... \oplus x_k' \oplus ... \oplus x_n \\
=& s \oplus x_k \oplus (x_k \oplus s) = 0
\end{aligned}
$$

#### Nguyên lí 3: Từ $\mathcal{P}$ chỉ đến $\mathcal{N}$
**Chứng minh**:  
Nếu ban đầu $s=0$, mọi nước đi hợp lệ (lấy ít nhất 1 sỏi từ 1 đống) sẽ làm thay đổi ít nhất 1 bit ⇒ $s'≠0$.

#### Ví dụ Minh họa
**Trạng thái**: $7, 5, 3, 6$
$7 = 0111$
$5 = 0101$
$3 = 0011$
$6 = 0110$
$s = 7⊕5⊕3⊕6 = 0111 (7)$
**Nước đi thắng**: Chọn đống 7 (có bit cao nhất = 1)
- $x' = 7⊕7 = 0$
- Trạng thái mới: $0, 5, 3, 6$ có $0⊕5⊕3⊕6 = 0$

#### Hệ quả
1. Người đi đầu có chiến thắng nếu trạng thái ban đầu ∈ $\mathcal{N}$
2. Chiến lược tối ưu: Luôn chuyển trò chơi về trạng thái ∈ $\mathcal{P}$

### E. Định lý Sprague - Grundy
Từ định lý Bouton, ta có thể mở rộng ra cách giải cho bài toán Nim khi xuất hiện một điều kiện $C$ nào đó. Tuy nhiên ta cần thêm một giá trị để đại diện cho số 
lượng sỏi để thỏa mãn ràng buộc. 

#### Phát biểu:
Gọi $g(x)$ là giá trị đại diện cho $x$. Ta có $g(x) = mex(\{g(y) : x \rightarrow y\})$. ($mex(S)$ (minimum excludant of $S$) là số nguyên không âm nhỏ nhất không xuất hiện trong tập $S$).

**Giải thích**
Vì sao lại là hàm $mex$? Nhận thấy trong bài toán cơ bản, $g(x) = x$ thỏa mãn các tính chất của hàm $mex$:
    Đặt $S(x) = \{g(y) : x \rightarrow y \}$. Trong bài toán cơ bản, $S(x) = \{g(0), g(1), ... g(x - 1)\}$. Nhận thấy $g(x) = mex(S(x))$.
    
**Chứng minh**
- Để thỏa mãn định lý Bouton, với mọi $k < g(x)$ phải tồn tại $y$ thỏa mãn $x \rightarrow y$ và $g(y) = k$. Vậy $g(x) \leq mex(S(x))$. $(1)$
- Mặt khác, không tồn tại $y$ thỏa mãn $x \rightarrow y$ và $g(y) = g(x)$. Vậy $g(x) \geq mex(S(x))$. $(2)$
Từ $(1)$ và $(2)$ suy ra $g(x) = mex(S(x))$.
    
Việc chứng minh định lí Sprague - Grundy rõ ràng bằng toán sẽ rất dài dòng, nên việc chứng minh cụ thể có thể được tham khảo ở [wiki](https://en.wikipedia.org/wiki/Sprague–Grundy_theorem) thay vì trình bày tại đây.
    
## Ứng dụng
Định lý này mở rộng cho nhiều biến thể trò chơi và là nền tảng của lý thuyết trò chơi tổ hợp.

## Bài tập
### Bài tập quy hoạch động trò chơi
- [Codeforces - 917B](https://codeforces.com/problemset/problem/917/B)
- [VNOI - Cuộc đấu cân não](https://oj.vnoi.info/problem/ncob)
- [Codeforces - 1215D](https://codeforces.com/problemset/problem/1215/D)
- [Codeforces - 1033C](https://codeforces.com/problemset/problem/1033/C)
### Bài tập toán bất biến
- [AtCoder - CaDDi2018 D](https://atcoder.jp/contests/caddi2018b/tasks/caddi2018_b)
- [Codeforces - 1383B](https://codeforces.com/problemset/problem/1383/B)
- [Codeforces Gym - 101808I](https://codeforces.com/gym/101808/problem/I)
### Bài tập Nim
- [MarisaOJ - Bốc sỏi](https://marisaoj.com/problem/620)
- [MarisaOJ - Bốc sỏi?](https://marisaoj.com/problem/623)
- [Codeforces - 2004E](https://codeforces.com/problemset/problem/2004/E)
- [CSES - Number Grid](https://cses.fi/problemset/task/1157)
