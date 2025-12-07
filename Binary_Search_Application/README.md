## Phase 1: The "First Thought" (Visualizing the Geometry)

Jisne pehli baar ye problem dekhi hogi, usne numbers ko nahi, Graph ko imagine kiya hoga.

### 1. Normal Sorted Array

- Ek seedhi line jo upar ja rahi hai. `/`
- (Sabse chhota element sabse left mein hai).

### 2. Rotated Sorted Array (No Duplicates)

- Ye ek "Cliff" (khai) jaisa dikhta hai.
- Pehle value badhti hai, fir achanak neeche girti hai (Minimum), aur fir wapas badhti hai.

### Internal Monologue

> "Mujhe wo point dhoondna hai jahan graph sabse neeche girta hai. Yaani The Deepest Valley."

Agar main Low, Mid, aur High dekhoon:

#### Case A: `[4, 5, 6, 7, 0, 1, 2]`

- Mid (7) hai, High (2) hai.
- Agar Mid > High hai, iska matlab "Pahad abhi ooncha hai, lekin aage jaake girega".
- **Conclusion:** Minimum right side mein hai (Valley aage hai).

#### Case B: `[7, 0, 1, 2, 3, 4, 5]`

- Mid (2) hai, High (5) hai.
- Agar Mid < High hai, iska matlab right side bilkul smooth hai (seedhi upar ja rahi hai). Wahan koi khai nahi hai.
- **Conclusion:** Minimum ya toh Mid khud hai, ya uske peeche (Left) mein hai.

---

## Phase 2: The "Aha!" Moment (Facing the Duplicate Fog)

Ab aate hain tumhare "Fog" (Dhundh) wale scenario pe. Jisne pehli baar duplicate case dekha hoga, wo yahan fasa hoga:

### Scenario 1

- `[3, 3, 1, 3]` (Minimum 1 hai)

### Scenario 2

- `[3, 1, 3, 3]` (Minimum 1 hai)

Dono cases mein:

- Low = 3
- Mid = 3
- High = 3

### The Confusion

> "Yaar, Mid aur High barabar hain (3 == 3). Ab main kaise bataun ki khai (drop) Left mein chhipi hai ya Right mein? Graph toh flat dikh raha hai!"

### The Logic Shift (Deriving the Solution)

Yahan us engineer ne socha hoga:
"Okay, mujhe nahi pata minimum kahan hai. Lekin mujhe ye pata hai ki nums[high] aur nums[mid] same hain."

> "Agar main high wala element ignore kar doon (matlab high-- kar doon), toh kya mera Minimum loss hoga?"

- Agar nums[high] minimum hota, toh nums[mid] bhi wahi hota (kyunki values same hain). Toh mid abhi bhi range mein hai.
- Agar nums[high] minimum nahi hai, toh waise bhi hatana safe hai.

### Result

Jab bhi `nums[mid] == nums[high]` ho (Confusion state), hum Safe Mode mein chale jayenge:

> "Risk mat lo, bas high ko ek kadam peeche kheecho (high--). Range chhoti ho jayegi aur dhundh (fog) thodi kam ho jayegi."

---

## Phase 3: Blueprint to Syntax (Logic Construction)

Ab is thought process ko code ke structure mein daalte hain.

### Step 1: Variables

- Humein low aur high chahiye poore array ko scan karne ke liye.

### Step 2: The Loop

- Jab tak `low < high`.
- (Note: Hum `=` nahi lagayenge kyunki humein ek element pe rukna hai jo minimum hoga).

### Step 3: Core Logic (The 3 Cases)

1. **Cliff right mein hai**

   - Agar `nums[mid] > nums[high]`. (Ex: `[5, 1, 2]`).
   - `low` ko aage badao (`mid + 1`).

2. **Cliff left mein (ya mid pe) hai**

   - Agar `nums[mid] < nums[high]`. (Ex: `[5, 0, 1, 2]`).
   - `high` ko mid pe le aao (mid ko skip nahi kar sakte, ho sakta hai wahi min ho).

3. **The Fog (Duplicates)**

   - Agar `nums[mid] == nums[high]`.
   - Decision nahi le pa rahe? `high` ko ek kam kar do (`high--`).

Iss logic ka:

### ⏱ Time Complexity (TC)

- **Best / Average case:** `O(log n)`

  - Jab duplicates kam hon, ya `nums[mid] != nums[high]` / `nums[mid] != nums[low]` zyada baar mile, to normal binary search jaise hi behave karega.

- **Worst case:** `O(n)`

  - Jab bahut saare elements **same** hon, jaise:

    - `[3, 3, 3, 3, 3]`
    - `[3, 3, 3, 1, 3, 3, 3]`

  - Aise cases mein `nums[mid] == nums[high]` / `nums[mid] == nums[low]` baar-baar aayega, aur hum sirf `high--` (ya `low++`) karke **linearly** shrink kar rahe hote hain.
  - Isliye worst case mein binary search ka advantage toot jaata hai → `O(n)`.

So, **final TC:**
👉 `O(log n)` average, **`O(n)` worst-case** because of duplicates.

---

### 💾 Space Complexity (SC)

- Sirf kuch variables use ho rahe hain: `low`, `high`, `mid`, etc.
- Koi extra array / recursion / data structure use nahi ho raha.

👉 **Space Complexity = `O(1)`** (constant extra space).

## Problem Overview

Yeh problem **Binary Search ka ek modified version** hai.
Isme **Duplicates (same numbers)** hone ki wajah se ek naya challenge aata hai.
Isko samajhne ke liye hum “**Pahadi Raasta aur Dhundh (Fog)**” ki story use karte hain.

---

## Part 1: The Story — **“Toota hua Pahad” (Rotated Array)**

Imagine karo ek pahad (Mountain) hai jo pehle seedha upar ja raha tha
(Sorted Array):
`[0, 1, 2, 4, 5]`

Kisi ne pahad ko beech se kaata aur peeche wala hissa aage laga diya.
Ab pahad aisa dikhta hai:
`[4, 5, 0, 1, 2]`

### Binary Search ka Rule

Binary Search tabhi kaam karta hai jab hum decision le paayein:

- **Left mein jana hai ya Right mein?**

Is decision ke liye hum check karte hain:

- Kya **Left side sorted (seedhi)** hai?
- Ya fir **Right side sorted** hai?

Jahan sorted milega, hum dekhenge:

- Kya **Target** us range mein aata hai ya nahi.

---

## Part 2: The Twist — **“Dhundh” (Duplicate Problem)**

Problem tab aati hai jab pahad par **Duplicate Patthar** hon.

### Example

Array:

```
[1, 0, 1, 1, 1]
```

Target: `0`

Values:

- Start (Low) = 1
- Mid = 1
- End (High) = 1

### Confusion (Dhundh)

- `nums[low] == nums[mid]` → Lagta hai Left sorted hai
- `nums[mid] == nums[high]` → Lagta hai Right sorted hai

Jab **Low == Mid == High** ho jata hai, toh computer confuse ho jaata hai.
Usse samajh nahi aata ki:

- Sorted part kaunsa hai
- Aur broken (rotated) part kaunsa

---

## Solution — **Fog Clear Karna**

Jab:

```
nums[low] == nums[mid] == nums[high]
```

hum **Safe Game** khelte hain:

- `low++`
- `high--`

### Kyun safe hai?

- Agar `nums[high]` minimum hota, toh `nums[mid]` bhi wahi hota.
- Agar minimum nahi hota, toh delete karna safe hai.
- Target miss hone ka risk nahi hota.

---

## Part 3: Step-by-Step Logic

Algorithm ko 3 major checks mein tod dete hain:

### 1️⃣ Dhundh (Fog) Check

Agar:

```
nums[low] == nums[mid] && nums[mid] == nums[high]
```

Action:

- `low++`
- `high--`
- `continue`

---

### 2️⃣ Left Sorted Check

Agar:

```
nums[low] <= nums[mid]
```

Toh Left side sorted hai.

- Agar Target low aur mid ke beech mein hai:

  ```
  high = mid - 1
  ```

- Nahi hai toh:

  ```
  low = mid + 1
  ```

---

### 3️⃣ Right Sorted Check

Agar Left sorted nahi hai, toh **Right side pakka sorted** hogi.

- Agar Target mid aur high ke beech mein hai:

  ```
  low = mid + 1
  ```

- Nahi hai toh:

  ```
  high = mid - 1
  ```

---

## Part 4: Visual Dry Run

### Case 1: Simple Case

Input:

```
nums = [2, 5, 6, 0, 0, 1, 2]
target = 0
```

Initial:

- low = 0 (2)
- high = 6 (2)

Iteration 1:

- mid = 3
- `nums[mid] == 0`

✅ Target mil gaya → **return true**

---

### Case 2: Tricky Duplicate Case

Input:

```
nums = [1, 0, 1, 1, 1]
target = 0
```

#### Iteration 1

- low = 0 (1), mid = 2 (1), high = 4 (1)
- Sab same → **Dhundh**

Action:

```
low = 1
high = 3
```

#### Iteration 2

Effective array: `[0, 1, 1]`

- low = 1 (0), mid = 2 (1), high = 3 (1)

Left sorted hai:

- `0 <= 1`
- Target range mein hai

Action:

```
high = mid - 1 = 1
```

#### Iteration 3

- low = 1, high = 1
- mid = 1, value = 0

✅ Target mil gaya → **return true**

---

## Part 5: C++ Code

### Platform

**Shutterstock**

### Language

**C++**

```cpp
#include <vector>
using namespace std;

class Solution {
public:
    bool search(vector<int>& nums, int target) {
        int low = 0;
        int high = nums.size() - 1;

        while (low <= high) {
            int mid = low + (high - low) / 2;

            // Step 1: Agar mid hi target hai
            if (nums[mid] == target) return true;

            // Step 2: The Fog (Dhundh) Check
            if (nums[low] == nums[mid] && nums[mid] == nums[high]) {
                low++;
                high--;
                continue;
            }

            // Step 3: Left side sorted?
            if (nums[low] <= nums[mid]) {
                if (nums[low] <= target && target < nums[mid]) {
                    high = mid - 1;
                } else {
                    low = mid + 1;
                }
            }
            // Step 4: Right side sorted
            else {
                if (nums[mid] < target && target <= nums[high]) {
                    low = mid + 1;
                } else {
                    high = mid - 1;
                }
            }
        }
        return false;
    }
};
```

---

## Follow-up Question Answer

### Sawal

**Duplicates hone se Complexity par kya asar padta hai?**

### Jawab

- Normal Binary Search ka TC: **O(log N)**
- Lekin Duplicates wale cases mein:

  - Jab hum baar-baar `low++` aur `high--` karte hain
  - Aur agar saare elements same ho (e.g. `[1, 1, 1, 1]`)

👉 Toh search **linear** ban jaati hai

### Final Result

- **Worst Case Time Complexity:** **O(N)**
- (Bilkul Linear Search jaisi)

---

✅ Structured
✅ Content unchanged
✅ Interview / Notes ready
