# ✅ Test Completion Report

![Status](https://img.shields.io/badge/Status-Completed-green)

---

## 🧠 Logic Test: 100%
**All tasks for the Logic Test have been successfully completed!** 🎉

### 📌 Completed Tasks:
- ✅ **Task 1:** Completed
```csharp
//Rules:
//Library:
/*
 Case1: if len s1 < len s2 => s1 < s2 <=> not the case
 Case2:  ab < bb because a < b
 Case3:  aabc < aacc => if same len, same prefix => compare the different part
*/

//ASCII compare string => ascII a -> z lower case 
//Problem: "ab" = "ba" => if only += (int)char 


//Solution: i: maxValue * 10 + a => 0*10 + a, a * 10 + b => ab => ab is different ba
//Brute force
// Time complexity: O(n^2) : Compare each case of delete 
// Space complexity O(1):

/*string result = "";
int minValue = int.MaxValue;

for (int i = 0; i < s.Length; i++)
{
    int sum = 0;
    string str = "";
    for (int j = 0; j < s.Length; j++)
    {
        if (i != j) //Remove char in i index => do not take to account
        {
            str += s[j];
            sum  = sum*10 + s[j];
        }
    }
     if (sum < minValue)
    {
        minValue = sum;
        result = str;
    }
}

return result;
*/

//Method #2: Greedy => eliminate where char > large 
// Time complexity: O(n)
// Space complexity O(1):

for (int i = 0; i < s.Length -1; i++)
{
    if (s[i] > s[i + 1])
    {
        return s.Remove(i, 1);
    }
    
}
return s.Remove(s.Length - 1, 1);
```
- ✅ **Task 2:** Completed
```csharp
public int Solution(int[][] A)
{
    //Method #1: Brute force: 2 Loops select first number, 2 loops select another number
    //Time complexity: O(n^4) 
    //Space complexity: O(1) 
    
    /*int maxVal = int.MinValue;
    int n = ar.Length; //Row
    int m = ar[0].Length; //Col

    for (int i = 0; i < n; i++) 
    {
        for (int j = 0; j < m; j++)
        {
            for (int k = 0; k < n; k++)
            {
                for (int q = 0; q < m; q++)
                {
                    if (i != k && j != q)
                    {
                        if(ar[i][j] + ar[k][q] > maxVal)
                        {
                            maxVal = ar[i][j] + ar[k][q]; 
                        }
                    }
                }
            }
        }
    }
    
    return maxVal;
    */
    
    //Method #2: Greedy
    //(value, col index) largest value of each row: {1,4}, {2,3} pick {4, 3}
    // => IF PICK 4 => 2 
    // => If pick 3 => 1 
    // => Pick 4 + 2 = 6 => largest
    // If 4,3 != col index => pick 4,3 
    // if 4 and 3 has same col index => Greedy => pick second largest
    
    //Time Complexity: O(n^2 + n^2) = O(2n^2) = O(n^2)
    //Space Complexity: O(3n) = O(n)

    int n = A.Length;
    int m = A[0].Length;
    
    (int, int, int)[] rows = new (int, int, int)[n]; // (Max Row, Second Row, Col index of max row)

    for (int i = 0; i < n; i++)
    {
        int maxRow = int.MinValue;
        int secondRow = int.MinValue;
        
        for (int j = 0; j < m; j++)
        {
            if (A[i][j] > maxRow)
            {
                secondRow = maxRow;
                maxRow = A[i][j];
                
                rows[i].Item1 = A[i][j];
                rows[i].Item3 = j;

            }else if (A[i][j] > secondRow)
            {
                secondRow = A[i][j];
            }
        }
        rows[i].Item2 = secondRow;
    }
    
    int result = int.MinValue;
    for (int i = 0; i < n; i++)
    {
        for (int j = i + 1; j < n; j++)
        {
            if (rows[i].Item3 != rows[j].Item3) //Diffeern col => save to choose
            {
                result = Math.Max(result, rows[i].Item1 + rows[j].Item1);
            }
            else //If same choose second largest in that row
            {
                result = Math.Max(result, rows[i].Item1 + rows[j].Item2);
            }
        }
    }
    return result;
}
```
- ✅ **Task 3:** Completed
```csharp
 public int Solution(int[] A)
{
    //Method #1: Greedy
    // Sort first O(nlogn), then compare to expected number
    // Because number restrict in range [1,N], which mean if if ar.len = 6 => then ideal array should be
    // {1,2,3,4,5,6}
    // Calculate difference between current value to the expected value then add to move
    
    // Time complexity: O(n) + O(nlogn) = O(nlogn)
    // Space complexity: O(1)
   
    /*Array.Sort(ar);
    int expected = 1;
    int cnt = 0;

    for (int i = 0; i < ar.Length; i++)
    {
        if (ar[i] != expected)
        {
            cnt += Math.Abs(ar[i] - expected);
        }
        expected++;
    }

    if (cnt > 1 * Math.Pow(10,9))
    {
        return -1;
    }
    return cnt;
    */
    
    
    //Method #2: Hashstable && Greedy
    // Time complexity: O(3n) = O(n)
    // Space complexity: O(n^2) worst case, average case O(n)
    
    Dictionary<int, int> ocurrences = new Dictionary<int, int>();

    for (int i = 0; i < A.Length; i++)
    {
        if (!ocurrences.ContainsKey(i+1))
        {
            ocurrences.Add(i+ 1, 0);
        }
        
        if (ocurrences.ContainsKey(A[i]))
        {
            ocurrences[A[i]]++;
        }
        else
        {
            ocurrences.Add(A[i], 1);
        }
    }

    int cnt = 0;
    
    //Because small value should be remapped to small value => minimum steps
    Queue<int> st = new Queue<int>();
    for (int i = 0; i < A.Length; i++)
    {
        if (ocurrences[i + 1] == 0)
        {
            st.Enqueue(i+1);
        }
    }

    for (int i = 0; i < A.Length; i++)
    {
        if(ocurrences[i + 1] > 1 && st.Count > 0)
        {
            ocurrences[i + 1]--;
            cnt += Math.Abs(i+1 - st.Dequeue());
        }
    }
    return cnt;
}
```

---

## 🎮 Unity Test: 100%
**All tasks for the Unity Test have been successfully completed!** 🚀

### 📌 Completed Tasks:
- ✅ **Task 1:** Completed
- ✅ **Task 2:** Completed
- ✅ **Task 3:** Completed
