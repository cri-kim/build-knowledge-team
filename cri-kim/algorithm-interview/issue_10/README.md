# 문자열조작

## 유효한 팰린드롬(Valid Palindrome)

> 회문(palindrome)이 하나 주어진다.

> 영문자와 숫자를 남겨두고 제거한다.

> 대문자를 소문자로 변경한다.

> 도출된 문자가 회문이면 true를 반환하고 아니면 false를 반환한다.

 

> 회문은 거꾸로 읽어도 제대로 읽는 것과 같은 문장이나 낱말, 숫자, 문자열 등이다.

 

### 동작

1. 영문자와 숫자를 남겨두고 제거한다.

2. 대문자를 소문자로 변경한다.

3. 문자를 반전시킨다.

4. 2번과 3번이 동일한지 확인한다. 

 

### 내용

 - 정규식(regular Expression)을 사용하여 영문자와 숫자를 제외하고 내용을 제거하였다.

 - 자바 String 에서 제공하는 toLowerCase() 메소드를 활용하여 소문자로 변경하였다.

 - StringBuffer에서 제공하는 reverse()메소드를 사용하여 글자를 반전 시켰다.

 - StringBuffer는 mutable class 이다. 버퍼에 공간을 두고 생성한 인스턴스의 값을 변경할 수 있다.

 - String 은 immuatalbe class 이다. 1-2번을 진행한 값은 추후 비교구문을 위해 사용되기 때문에 String을 사용하였다.

 

### 소스 코드

```

public boolean isPalindrome(String s) {

    String defaultString = s.replaceAll("[^a-zA-Z0-9]","").toLowerCase();

    StringBuffer sb = new StringBuffer(defaultString).reverse();

 

    return defaultString.equals(sb.toString());

}

```

 

### 테스트 코드

```

@Test

void isPalindromeTest(){

    LeetCode125 leetCode125 = new LeetCode125();

    assertThat(leetCode125.isPalindrome("0P")).isFalse();

    assertThat(leetCode125.isPalindrome("race a car")).isFalse();

    assertThat(leetCode125.isPalindrome("A man, a plan, a canal: Panama")).isTrue();

}

```

 

## 문자열 뒤집기(Reverse String)

> 문자를 배열로 읽어 해당 뒤집어서 해당 배열로 반환하시오.

> 단, 문자열 재배치를 위해 다른 배열 객체를 생성하지마세요.

### 동작

1. 배열을 순차적으로 읽으며 시작과 끝의 배열 값을 swap 한다.

 

### 내용

 - 배열은 생성시 자료형의 크기만큼 배열의 갯수에 따른 메모리 크기가 결정된다. (스택 메모리에 실제값이 저장된다.)

 - 배열의 값을 변경하면 새로운 배열이 생성되는 것이 아닌 해당 배열의 데이터가 변경이 된다.

 - privitive type 에 대해 알아보면 좋다.

```

public void reverseString(char[] s) {

    int len = s.length;

    if(len == 0) return;

    for (int i=0; i < (len/2); i++)

    {

        char l = s[i];

        s[i] = s[len-i-1];

        s[len-i-1] = l;

    }

}

```

 

### 테스트 코드

```

@Test

public void reverseStringTest() {

    LeetCode344 let = new LeetCode344();

    char[] s = new char[]{'h','e','l','l','o'};

    let.reverseString(s);

    assertEquals(s[0], 'o');

}

```

## 로그 파일 재정렬(Reorder Data in Log Files)

> 로그 배열이 주어진다. 로그는 space(공백)으로 구분지어져 있는 string 단어들이다.

> 첫번째 단어는 모두 identifier이다.

 

> 로그의 타입은 문자(letter), 숫자(digit) 타입 두가지가 있다.

 

1. 문자 로그는 무조건 숫자 로그 앞에 배치하게된다.

2. 문자 로그는 사전식 순서에 따라 정렬된다.

3. 만약 비교하는 두 문자 로그의 사전식 순서가 동일하다면, identifier의 사전식 순서를 비교한다.

4. 숫자 로그는 기존의 순서를 유지한다.

 

### 내용

 - 로그의 분리 및 관리를 위해 로그 Class를 별도로 만들었다.

	- origin 값을 유지하기 위해 defaultString, id와 로그의 분리를 위해 values를 만들었다.

	- 비교식을 사용자 정의에 맡기기 위해서 Comparable를 상속받았다. 이에 따라서 정렬의 요구사항 1~4번을 적용하였다.

 - 문자/숫자 로그에 대한 분리를 진행하였다.

 - 정렬이 필요한 문자 로그 리스트를 커스텀한 정의에 따라서 정렬시켰다.

 - 문자/숫자 로그 리스트를 합친 후, 요구사항에 맞춘 String 배열 형식으로 반환하였다.

 - 이 문제를 해결하기 위해서 Comparable, Comparator에 대해 학습하였다.

 - String compare을 위해 사용한 메소드는 Comparator를 상속한 것으로 두 값의 비교를 할 수 있다. 또한, 상속받아 커스텀할 수 있다.

 - Comparable의 경우 현재 객체와 비교대상의 객체 값을 비교할 수 있다.

 - 두 클래스 모두 비교 구문 사용에 있어서 주의를 기울여야 한다. 특히, int 반환이기 때문에 overflow/underflow에 대한 고려가 필요하다.

 

### 소스코드

```

public class LeetCode937 {

public String[] reorderLogFiles(String[] logs) {

    List<Log> letters = new ArrayList<>();

    List<Log> digits = new ArrayList<>();

    for(String log : logs){

        Log newLog = new Log(log);

        if(newLog.isDigit()){

            digits.add(newLog);

        }

        else{

            letters.add(newLog);

        }

    }

    Collections.sort(letters);

    List<String> rs = new ArrayList<>();

    for(Log log : letters){

        rs.add(log.defaultString);

    }

    for(Log log : digits){

        rs.add(log.defaultString);

    }

 

    return rs.toArray(new String[rs.size()]);

}

}

class Log  implements Comparable<Log>{

    private static final String DELIMETER = " ";

    String[] values;

    String defaultString;

    String id;

    boolean isDigit=false;

 

    public Log(String defaultString){

        this.defaultString = defaultString;

        this.values = defaultString.split(DELIMETER);

        this.id = values[0];

    }

 

    @Override

    public int compareTo(Log o) {

        if(isDigit && o.isDigit) {

            return 0;

        }

        if(getContents().equals(o.getContents())){

            return String.CASE_INSENSITIVE_ORDER.compare(id, o.id);

        }

        return String.CASE_INSENSITIVE_ORDER.compare(getContents()

                , o.getContents());

    }

    public String getContents(){

        return String.join(" "

                ,Arrays.copyOfRange(values, 1, values.length));

    }

 

    public boolean isDigit() {

        try {

            Double.parseDouble(getContents().trim());

        } catch (NumberFormatException e){

            return false;

        }

        isDigit = true;

        return true;

    }

}

```

### 테스트 코드

```

@Test

void reorderLogFilesTest(){

    String[] defaultArray = new String[]{"dig1 8 1 5 1","let1 art can","dig2 3 6","let2 own kit dig","let3 art zero"};

    LeetCode937 method = new LeetCode937();

    String[] expectedArray = method.reorderLogFiles(defaultArray);

    assertEquals(expectedArray[0],"let1 art can");

}

```

## 가장 흔한 단어

> 하나의 단락이 String으로 주어지고, 금지된 단어가 String 배열로 주어진다.

> 금지된 단어를 제외하고 가장 많이 사용된 단어를 반환한다.

 

### 내용

 - 단어의 노출 횟수를 저장하기 위해 Map을 만들었다. Map은 Key, Value로 이루어진 자료형이며, key값은 중복될 수 없다.

 - banned를 탐색하기 쉽게 Set에 넣었다. Set은 중복되지 않는 값으로 만들어진 자료형이다.

 - 정규표현식을 이용하여 단락의 단어를 분리하고 각 단어의 사용 횟수를 map에 저장한다.

 - 가장 횟수가 많은 단어를 반환한다.

### 소스코드

```

private static final String DELIMETER = "[^a-zA-Z]|\\s";

public String mostCommonWord(String paragraph, String[] banned) {

    Map<String, Integer> map = new HashMap<String,Integer>();

    Set<String> set = Set.of(banned);

    for(String s : paragraph.split(DELIMETER)){

        String word = s.toLowerCase();

        if(set.contains(word)||s.isBlank()){

            continue;

        }

        int cnt = (map.containsKey(word) ? map.get(word) : 0) + 1;

        map.put(word, cnt);

    }

 

    Optional<Map.Entry<String, Integer>> max

            = map.entrySet().stream().max(Map.Entry.comparingByValue());

    return max.get().getKey();

}

```

 

### 테스트 코드

```

@Test

void mostCommonWordTest(){

    String paragraph = "Bob hit a ball, the hit BALL flew far after it was hit.";

    String[] banned = new String[]{"hit"};

    LeetCode819 method = new LeetCode819();

    assertEquals(method.mostCommonWord(paragraph, banned), "ball");

    assertEquals(method.mostCommonWord("a, a, a, a, b,b,b,c, c", new String[]{"a"}), "b");

}

```

## 그룹 애너그램(Group Anagrams)

### 동작

- 사전식으로 문자열을 정렬하는 메소드 생성

- 그룹인지 판별하는 메소드 생성 

- 같은 그룹일 경우 정렬된 문자를 key 값으로 하여 Map 에 저장

- 완료된 map을 리스트로 반환 한 후, 요소의 size 크기 순으로 정렬하여 반환

 

### 소스코드

```

public class LeetCode49 {

    public List<List<String>> groupAnagrams(String[] strings) {

        Map<String, List<String>> map = new HashMap<>();

        for(int i=0;i<strings.length;i++){

            String orderedString = getLxicographicalOrder(strings[i]);

            if(map.containsKey(orderedString)){

                map.get(orderedString).add(strings[i]);

                continue;

            }

            List<String> tempList = new ArrayList<>();

            tempList.add(strings[i]);

            map.put(orderedString, tempList);

        }

        List<List<String>> rs = new ArrayList<>(map.values());

        rs.sort(Comparator.comparing(List::size));

        return rs;

    }

 

    public boolean isGroup(String s, String s2) {

        return getLxicographicalOrder(s).equals(getLxicographicalOrder(s2));

    }

    public String getLxicographicalOrder(String s){

        char[] chars = s.toCharArray();

        Arrays.sort(chars);

        return new String(chars);

    }

}

```

### 테스트 코드

```

@Test

void groupAnagramsTest(){

    LeetCode49 method = new LeetCode49();

    List<List<String>> expectedList = method.groupAnagrams(new String[]{"eat","tea","tan","ate","nat","bat"]});

 

    assertEquals(expectedList.get(0).get(0),"bat");

}

 

@Test

void isGroupTest(){

    LeetCode49 method = new LeetCode49();

    assertEquals(method.isGroup("nat","tan"),true);

    assertEquals(method.isGroup("nat","tane"),false);

}

@Test

void getLxicographicalOrderTest(){

    LeetCode49 method = new LeetCode49();

    assertEquals(method.getLxicographicalOrder("bac"),"abc");

}

```

## 가장 긴 팰린드롬 부분 문자열

 

### 동작1 ( ***시간 초과*** )

- 모든 경우의 수를 탐색 및 회문 판별하여 가장 긴 길이를 반환

 

### 소스코드

```

public boolean isPalindrome(String s) {

    return new StringBuffer(s).reverse().toString().equals(s);

}

 

public String longestPalindrome(String s) {

    StringBuffer sb = new StringBuffer(s);

    String longestString=sb.substring(0,1);

    for(int i=0;i<s.length();i++){

        for(int j=i;j<s.length()+1;j++){

            String temp = sb.substring(i,j);

            if(isPalindrome(temp) && longestString.length() < temp.length()){

                longestString = temp;

            }

        }

    }

    return longestString;

}

```

### 테스트 코드

```

@Test

void longestPalindromeTest(){

    LeetCode5 method = new LeetCode5();

    assertEquals(method.longestPalindrome("babad"),"bab");

    assertEquals(method.longestPalindrome("cbbd"),"bb");

    assertEquals(method.longestPalindrome("a"),"a");

    assertEquals(method.longestPalindrome("ac"),"a");

}

@Test

void isPalindromeTest(){

    LeetCode5 method = new LeetCode5();

    assertEquals(method.isPalindrome("bab"), true);

}

```

### 동작2 ( ***python 답안 확인 후 재구성*** )
- 회문 판별 메소드 생성
- expand 메소드로 최대 길이에서 양쪽(좌측/우측)의 값을 줄이며 회문인지 판별
- for문으로 전체 비교

### 소스코드
```
public String longestPalindrome(String s) {
        if(s.length() < 2 || isPalindrome(s)){
            return s;
        }
        String rs = "";
        for(int i=0;i<s.length() -1;i++){
            String ex1 = expand(i,i+1,s);
            String ex2 = expand(i,i+2,s);
            if(ex1.length() > ex2.length()){
                if(ex1.length() > rs.length()){
                    rs = ex1;
                }
            }
            else{
                if(ex2.length() > rs.length()){
                    rs = ex2;
                }
            }
        }
        return rs;
    }
    public String expand(int left, int right, String s){
        while(left >= 0 && right< s.length() && s.charAt(left) == s.charAt(right)){
            left--;
            right++;
        }
        return s.substring(left+1, right);
    }

    public boolean isPalindrome(String s) {
        StringBuffer sb = new StringBuffer(s);
        return sb.reverse().toString().equals(s);
    }
```
### 테스트 코드
```
    @Test
    void longestPalindromeTest(){
        LeetCode5 method = new LeetCode5();
        assertEquals("bab", method.longestPalindrome("babad"));
        assertEquals("bb",method.longestPalindrome("cbbd"));
        assertEquals("cc",method.longestPalindrome("ccd"));
    }

    @Test
    void isPalindromeTest(){
        LeetCode5 method = new LeetCode5();
        assertEquals(method.isPalindrome("ada"),true);
        assertEquals(method.isPalindrome("adac"),false);
    }
```