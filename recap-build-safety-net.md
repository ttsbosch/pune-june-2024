### Switch Case Refactored

```CS
private static readonly string[] SoundexMapping = {
    "01230120022455012623010202", // A-M
    "00000000000000000000000000"  // N-Z
  };

 private static char GetSoundexCode(char c)
  {
    c = char.ToUpper(c);
    int index = c - 'A';
    if (index < 0 || index >= SoundexMapping.Length * SoundexMapping[0].Length)
    {
      return '0';
    }

    return SoundexMapping[index / 13][index % 13];
  }

```

```c
int case1() { return 1; }
int case2() { return 2; }
int case3() { return 3; }
int case5() { return 5; }
int case4() { return 4;}
int case6() { return 6;}
int defaultCase() { return 0; }

typedef struct {
    char value[10];
    int (*func)();
} CaseEntry;
.....
.....
int getSoundexCode(char letter) {
    static CaseEntry cases[] = {
        {{'B','F','P','V', '\0'}, case1},
        {{'C','G','J','K','Q','S','X','Z', '\0'}, case2},
        {{'D','T', '\0'}, case3},
        {{'M','N', '\0'}, case5},
        {{'A','E','I','O','U','H','W','Y', '\0'}, defaultCase},
        {{'L','\0'},case4},
        {{'R','\0'},case6}
    };

    for (int i = 0; i < 5; i++) {
        if(findInCase(cases[i].value, letter)){
            return cases[i].func();
        }
    }
}
```

```c

void generateSoundex(const char *name, char *soundex) {
    int len = strlen(name);
    soundex[0] = toupper(name[0]);
    int sIndex = 1;
    char arr[128];
    arr['B'] = '1';
    arr['F'] = '1';
    arr['P'] = '1';
    arr['V'] = '1';
    arr['C'] = '2';
    arr['G'] = '2'; arr['J'] = '2'; arr['K'] = '2'; arr['Q'] = '2'; arr['S'] = '2'; arr['X'] = '2'; arr['Z'] = '2';
    arr['D'] = '3'; arr['T'] = '3';
    arr['L'] = '4';
    arr['M'] = '5'; arr['N'] = '5';
    arr['R'] = '6';
    arr['A'] = '0'; arr['E'] = '0'; arr['I'] = '0'; arr['O'] = '0'; arr['U'] = '0'; arr['H'] = '0'; arr['W'] = '0'; arr['Y'] = '0';
    
    for (int i = 1; i < len && sIndex < 4; i++) {
        char code = arr[toupper(name[i])];
        if (code != '0' && code != soundex[sIndex - 1]) {
            soundex[sIndex++] = code;
        }
    }
    
    while (sIndex < 4) {
        soundex[sIndex++] = '0';
    }

    soundex[4] = '\0';
}

```

```c
char BFPV(char c){
    if(c=='B' || c=='F' || c=='P' || c=='V'){
        return '1';
    }
    return CGKKQSXZ(c);
}

char CGKKQSXZ(char c){
    if(c=='C' || c=='G' || c=='J' || c=='K' || c=='Q' || c=='S' || c=='X' || c=='Z'){
        return '2';
    }
    return  DT(c);
}

char DT(char c){
    if(c=='D' || c=='T'){
        return '3';
    }
    return L(c);
}


char L(char c){
    if(c=='L'){
        return'4';
    }
   return MN(c);
}


char MN(char c){
    if(c=='N' || c=='M'){
        return '5';
    }
    return R(c);
}


char R(char c){
    if(c=='R'){
        return '6';
    }
    return '0';
}


char getSoundexCode(char c) {
    c = toupper(c);
    return BFPV(c);
    
```
### Good Separation
```cs
 public static string GenerateSoundex(string name)
    {
        if (string.IsNullOrEmpty(name))
        {
            return string.Empty;
        }

        StringBuilder soundex = new StringBuilder();
        SoundexAppend(soundex, name[0]);
        GetSoundexProcess(soundex, name);

        checkingsoundex(soundex);

        return soundex.ToString();
    }

```


### Value Based Testing (Repeated)

```csharp
 [Fact]
    public void HandlesEmptyString()
    {
        Assert.Equal(string.Empty, Soundex.GenerateSoundex(""));
    }

    [Fact]
    public void HandlesSingleCharacter()
    {
        Assert.Equal("A000", Soundex.GenerateSoundex("A"));
    }

    [Fact]
    public void HandlesMultipleConsonantsAndVowels()
    {
        Assert.Equal("R163", Soundex.GenerateSoundex("Robert"));
    }
    
 
    [Fact]
    public void HandlesSpecialCharacters()
    {
        Assert.Equal("J530", Soundex.GenerateSoundex("John-Doe"));
    }


    [Fact]
    public void HandlesWithlowercaseCharacters()
    {
        Assert.Equal("A420", Soundex.GenerateSoundex("alice"));
    }

    [Fact]
    public void HandlesWithExactly_4_Characters()
    {
        Assert.Equal("A350", Soundex.GenerateSoundex("Adam"));
    }

    [Fact]
    public void HandlesWithMoreThan_4_Characters()
    {
        Assert.Equal("C623", Soundex.GenerateSoundex("Christopher"));
    }

    [Fact]
    public void HandlesLongString()
    {
        Assert.Equal("A123", Soundex.GenerateSoundex("abcdefghijklmnopqrstuvwxyz"));
    }
    
     [Fact]
    public void HandlesBothCharAndDigit()
    {
        Assert.Equal("J500", Soundex.GenerateSoundex("Jane123"));
    }   
```
### New Grammer but retains complexity

```csharp
public static char GetSoundexCode(char character)
{
    character = char.ToUpper(character);
    return character switch
    {
        'B' or 'F' or 'P' or 'V' => '1',
        'C' or 'G' or 'J' or 'K' or 'Q' or 'S' or 'X' or 'Z' => '2',
        'D' or 'T' => '3',
        'L' => '4',
        'M' or 'N' => '5',
        'R' => '6',
         _ => '0'
     };
}
}
```

### Time and Space Complexity
```c++
char getSoundexCode(char c) {
    c = toupper(c);
    std::map<char,char> charMap = { {'B','1'}, {'F','1'}, {'P','1'}, {'V','1'},
                           {'C','2'}, {'G','2'}, {'J','2'}, {'K','2'}, {'Q','2'}, {'S','2'}, {'X','2'}, {'Z','2'},
                           {'D','3'}, {'T','3'},
                           {'L','4'},
                           {'M','5'}, {'N','5'},
                           {'R','6'},
                           {'A','0'}, {'E','0'}, {'I','0'}, {'O','0'}, {'U','0'}, {'H','0'}, {'W','0'}, {'Y','0'}
                           };
    // return Map consonants to digits
    for (auto charValue : charMap) // Look at each key-value pair
    {
        if (charValue.first == c)
        {
            return charValue.second;
        }
    }
}
```

``` python
def get_soundex_code(c):
    c = c.upper()
    mapping = {
        "B": "1",
        "F": "1",
        "P": "1",
        "V": "1",
        "C": "2",
        "G": "2",
        "J": "2",
        "K": "2",
        "Q": "2",
        "S": "2",
        "X": "2",
        "Z": "2",
        "D": "3",
        "T": "3",
        "L": "4",
        "M": "5",
        "N": "5",
        "R": "6",
    }
    return mapping.get(c, "0")  # Default to '0' for non-mapped characters
 

```

``` python
def get_soundex_code(char):
    """
    Returns the Soundex code for the given character.
    """
    return {
        'B': '1', 'F': '1', 'P': '1', 'V': '1',
        'C': '2', 'G': '2', 'J': '2', 'K': '2', 'Q': '2', 'S': '2', 'X': '2', 'Z': '2',
        'D': '3', 'T': '3',
        'L': '4',
        'M': '5', 'N': '5',
        'R': '6'
    }.get(char.upper(), '0')
```
``` Java
public static char getSoundexCode(char c) {
		Map<Character, Character> characterMap = buildSoundexMap();
		if(characterMap.containsKey(Character.toUpperCase(c))) {
			return characterMap.get(Character.toUpperCase(c));
		}
		return '0';
	}

	public static Map<Character, Character> buildSoundexMap() {
		Map<Character, Character> characterMap = new HashMap<>();
		characterMap.putAll(populateSoundexMap(Arrays.asList('B', 'F', 'P', 'V'), '1'));
		characterMap.putAll(populateSoundexMap(Arrays.asList('C', 'G', 'J', 'K', 'Q', 'S', 'X', 'Z'), '2'));
		characterMap.putAll(populateSoundexMap(Arrays.asList('D', 'T'), '3'));
		characterMap.putAll(populateSoundexMap(Arrays.asList('L'), '4'));
		characterMap.putAll(populateSoundexMap(Arrays.asList('M', 'N'), '5'));
		characterMap.putAll(populateSoundexMap(Arrays.asList('R'), '6'));
		return characterMap;
	}

	public static Map<Character, Character> populateSoundexMap(List<Character> nameCharList, char code) {
		Map<Character, Character> characterMap = new HashMap<>();
		if(isEmptyList(nameCharList)) {
			for(Character nameChar: nameCharList) {
				characterMap.put(nameChar, code);
			}
		}
		return characterMap;
	}
```
### Good Separation  in Action
```C++
class Soundex {
public:
    // Constructor
    Soundex();

    // Generates the full Soundex code for a given name
    std::string generateSoundex(const std::string& name);

private:
    // Static array mapping characters to Soundex codes
    static const char soundexMap[26];

    // Get the Soundex code for a given character
    char getSoundexCode(char c) const;

    // Start the Soundex code with the first letter of the name
    std::string startSoundex(const std::string& name) const;

    // Generate the remaining Soundex code
    void generateRemainingSoundex(std::string& soundex, const std::string& name) const;

    // Process the current character in the name
    void processCurrentChar(std::string& soundex, char currentChar, char& prevCode, char& prevPrevChar) const;

    // Check if the code should be added
    bool shouldAddCode(char prevPrevChar, char currentChar) const;

    // Add the character to the Soundex code
    void processCharacter(std::string& soundex, char code, char& prevCode) const;

    // Check if the Soundex code is valid (not '0')
    bool isValidSoundexCode(char code) const;

    // Check if the code is different from the previous one
    bool isNewCode(char code, char prevCode) const;

    // Check if characters are separated by 'H' or 'W'
    bool isSeparatedByHorW(char prevPrevChar) const;

    // Pad the Soundex code with zeros to ensure it's 4 characters long
    void padWithZeros(std::string& soundex) const;

    // Check if the character is a vowel
    bool isVowel(char c) const;

    bool shouldContinueProcessing(size_t nameLength, size_t soundexLength) const;

    bool shouldAddCode(char currentCode, char prevCode, char prevPrevCode) const;

    bool isShortName(const std::string& name) const;
    bool canAddCode(char prevPrevCode, char currentCode) const ;
```
### UnNecessary Test Cases (Test public interface methods only)
```c
char arr_with_ret_1[4] = {'B','F','P','V'};
char arr_with_ret_2[8] = {'C','G','J','K','Q','S','X','Z'};
char arr_with_ret_3[2] = {'D','T'};
char arr_with_ret_4[1] = {'L'};
char arr_with_ret_5[2] = {'M','N'};
char arr_with_ret_6[1] = {'R'};


// Test cases for getSoundexCode function
TEST(SoundexTest_ret_1, GetSoundexCode)
{
    for (int i=0;i<4;i++)
    {
        EXPECT_EQ(getSoundexCode(arr_with_ret_1[i]), '1');
    }
}

TEST(SoundexTest_ret_2, GetSoundexCode)
{
    for (int i=0;i<8;i++)
    {
        EXPECT_EQ(getSoundexCode(arr_with_ret_2[i]), '2');
    }
}

```

```python
# Additional tests for internal functions
    def test_get_soundex_code(self):
        self.assertEqual(get_soundex_code('A'), '0')
        self.assertEqual(get_soundex_code('B'), '1')
        self.assertEqual(get_soundex_code('C'), '2')
        self.assertEqual(get_soundex_code('D'), '3')
        self.assertEqual(get_soundex_code('L'), '4')
        self.assertEqual(get_soundex_code('M'), '5')
        self.assertEqual(get_soundex_code('R'), '6')
        self.assertEqual(get_soundex_code('Z'), '2')
        self.assertEqual(get_soundex_code('a'), '0')  # Case insensitivity

    def test_validate_name(self):
        self.assertFalse(validate_name(""))
        self.assertTrue(validate_name("John"))

    def test_initialize_soundex(self):
        self.assertEqual(initialize_soundex("John"), "J")
        self.assertEqual(initialize_soundex("smith"), "S")

    def test_should_add_code(self):
        self.assertTrue(should_add_code('1', '0'))
        self.assertFalse(should_add_code('0', '0'))
        self.assertFalse(should_add_code('1', '1'))

    def test_handle_character(self):

        prev_code, soundex = handle_character('m', '0', 'S')
        self.assertEqual(prev_code, '5')
        self.assertEqual(soundex, 'S5')

    def test_process_characters(self):
        self.assertEqual(process_characters("Washington"), "W252")
        self.assertEqual(process_characters("Lee"), "L")

    def test_pad_soundex(self):
        self.assertEqual(pad_soundex("W252"), "W252")
        self.assertEqual(pad_soundex("L"), "L000")
```

```python
def get_soundex_code(c):
    c = c.upper()
    mapping = {
        'B': '1', 'F': '1', 'P': '1', 'V': '1',
        'C': '2', 'G': '2', 'J': '2', 'K': '2', 'Q': '2', 'S': '2', 'X': '2', 'Z': '2',
        'D': '3', 'T': '3',
        'L': '4',
        'M': '5', 'N': '5',
        'R': '6'
    }
    return mapping.get(c, '0')  # Default to '0' for non-mapped characters

def test_get_soundex_code(self):
        # Test for '1' mappings
        self.assertEqual(get_soundex_code('B'), '1')
        self.assertEqual(get_soundex_code('F'), '1')
        self.assertEqual(get_soundex_code('P'), '1')
        self.assertEqual(get_soundex_code('V'), '1')
        
        # Test for '2' mappings
        self.assertEqual(get_soundex_code('C'), '2')
        self.assertEqual(get_soundex_code('G'), '2')
        self.assertEqual(get_soundex_code('J'), '2')
        self.assertEqual(get_soundex_code('K'), '2')
        self.assertEqual(get_soundex_code('Q'), '2')
        self.assertEqual(get_soundex_code('S'), '2')
        self.assertEqual(get_soundex_code('X'), '2')
        self.assertEqual(get_soundex_code('Z'), '2')

        # Test for '3' mappings
        self.assertEqual(get_soundex_code('D'), '3')
        self.assertEqual(get_soundex_code('T'), '3')

        # Test for '4' mapping
        self.assertEqual(get_soundex_code('L'), '4')

        # Test for '5' mappings
        self.assertEqual(get_soundex_code('M'), '5')
        self.assertEqual(get_soundex_code('N'), '5')

        # Test for '6' mapping
        self.assertEqual(get_soundex_code('R'), '6')

        # Test for characters that should return '0'
        self.assertEqual(get_soundex_code('H'), '0')
        self.assertEqual(get_soundex_code('W'), '0')
        self.assertEqual(get_soundex_code('A'), '0')
        self.assertEqual(get_soundex_code('E'), '0')
        self.assertEqual(get_soundex_code('I'), '0')
        self.assertEqual(get_soundex_code('O'), '0')
        self.assertEqual(get_soundex_code('U'), '0')
        self.assertEqual(get_soundex_code('Y'), '0')
        self.assertEqual(get_soundex_code('1'), '0')
        self.assertEqual(get_soundex_code('*'), '0')

```

``` Python
class TestSoundex(unittest.TestCase):
    def test_get_soundex_code(self):
        """Test the get_soundex_code function"""
        self.assertEqual(get_soundex_code('B'), '1')
        self.assertEqual(get_soundex_code('F'), '1')
        self.assertEqual(get_soundex_code('P'), '1')
        self.assertEqual(get_soundex_code('V'), '1')
        self.assertEqual(get_soundex_code('C'), '2')
        self.assertEqual(get_soundex_code('G'), '2')
        self.assertEqual(get_soundex_code('J'), '2')
        self.assertEqual(get_soundex_code('K'), '2')
        self.assertEqual(get_soundex_code('Q'), '2')
        self.assertEqual(get_soundex_code('S'), '2')
        self.assertEqual(get_soundex_code('X'), '2')
        self.assertEqual(get_soundex_code('Z'), '2')
        self.assertEqual(get_soundex_code('D'), '3')
        self.assertEqual(get_soundex_code('T'), '3')
        self.assertEqual(get_soundex_code('L'), '4')
        self.assertEqual(get_soundex_code('M'), '5')
        self.assertEqual(get_soundex_code('N'), '5')
        self.assertEqual(get_soundex_code('R'), '6')
        self.assertEqual(get_soundex_code('z'), '2')
        self.assertEqual(get_soundex_code('1'), '0')
```

``` Js
module.exports = {
  getSoundexCode,
  generateSoundex
};

describe('Soundex Algorithm', () => {
  describe('getSoundexCode', () => {
    it('should return the correct code for consonants', () => {
      expect(getSoundexCode('B')).to.equal('1');
      expect(getSoundexCode('C')).to.equal('2');
      expect(getSoundexCode('D')).to.equal('3');
      expect(getSoundexCode('L')).to.equal('4');
      expect(getSoundexCode('M')).to.equal('5');
      expect(getSoundexCode('R')).to.equal('6');
    });

    it('should return 0 for vowels and ignored characters', () => {
      expect(getSoundexCode('A')).to.equal('0');
      expect(getSoundexCode('E')).to.equal('0');
      expect(getSoundexCode('I')).to.equal('0');
      expect(getSoundexCode('O')).to.equal('0');
      expect(getSoundexCode('U')).to.equal('0');
      expect(getSoundexCode('Y')).to.equal('0');
      expect(getSoundexCode('H')).to.equal('0');
      expect(getSoundexCode('W')).to.equal('0');
    });

    it('should be case-insensitive', () => {
      expect(getSoundexCode('b')).to.equal('1');
      expect(getSoundexCode('c')).to.equal('2');
      expect(getSoundexCode('d')).to.equal('3');
    });
  });

```

### FAT Interface
```C++
StrigCalculator.h
.....
.....
char getSoundexCode(char c);
bool isHW(char c);
bool shouldAppend(char code, char prevCode, char currentChar);
std::string accumulateSoundexCodes(const std::string& name);
std::string padSoundex(const std::string& soundex);
std::string generateSoundex(const std::string& name);

#endif // SOUNDEX_H
```
###  Time Complexity Code
```c
int LetterList(char letter, int row_value ,int column_value)
{
    char SetArray[6][8] = {
    {'B','F','P','V'},
    {'C','G','J','K','Q','S','X','Z'},
    {'D','T'},
    {'L'},
    {'M','N'},
    {'R'}
    };

     for (int i = 0; i < column_value; i++)
    {
        if (letter == SetArray[row_value][i])
        {
            return 1;
            break;
        }
    }
    return 0;
}


char ValueCheck6(char c)
{
    int row_value = 5;
    int column_value = 1;
    char letter = toupper(c);
    if (LetterList(letter,row_value,column_value) == 1)
    {
        return '6';
    }
    else
    {
        return '0';
    }
}


char ValueCheck5(char c)
{
    int row_value = 4;
    int column_value = 2;
    char letter = toupper(c);
    if (LetterList(letter,row_value,column_value) == 1)
    {
        return '5';
    }
    else
    {
        return ValueCheck6(c);
    }
}


char ValueCheck4(char c)
{
    int row_value = 3;
    int column_value = 1;
    char letter = toupper(c);
    if (LetterList(letter,row_value,column_value) == 1)
    {
        return '4';
    }
    else
    {
        return ValueCheck5(c);
    }
}


char ValueCheck3(char c)
{
    int row_value = 2;
    int column_value = 2;
    char letter = toupper(c);
    if (LetterList(letter,row_value,column_value) == 1)
    {
        return '3';
    }
    else
    {
        return ValueCheck4(c);
    }
}
char ValueCheck2(char c)
{
    int row_value = 1;
    int column_value = 8;
    char letter = toupper(c);
    if (LetterList(letter,row_value,column_value) == 1)
    {
        return '2';
    }
    else
    {
        return ValueCheck3(c);
    }
}


char ValueCheck1(char c)
{
    int row_value = 0;
    int column_value = 4;
    char letter = toupper(c);
    if (LetterList(letter,row_value,column_value) == 1)
    {
        return '1';
    }
    else
    {
        return ValueCheck2(c);
    }
}

char getSoundexCode(char c){ 
    return ValueCheck1(c);

}
```

### Where to find the Truth : In Code or Comment
```py
 def testcases(self):
        self.assertEqual(generate_soundex("Radhakrishnan"),"R326")  #Random name more than 4 characters
        self.assertEqual(generate_soundex("X"), "X000")            # Single character non-vowel
        self.assertEqual(generate_soundex("aeiou"),"A000")         # All vowels
        self.assertEqual(generate_soundex("Quite"), "Q300")        
        self.assertEqual(generate_soundex("Quiet"), "Q300")        # Similar sound 
        self.assertEqual(generate_soundex("A2B"), "A100")         # Non-alphabetic and pad with zeros
        self.assertEqual(generate_soundex("APPLE"), "A140")     # Repeating characters with vowels ignored
        self.assertEqual(generate_soundex("HARRYPOTTER"), "H613")     # Example with same consecutive letters, containing 'y' which should be ignored   
        self.assertEqual(generate_soundex("HANUMAN"), "H555")     # Example with same consecutive code separated by a vowel - coded twice
        self.assertEqual(generate_soundex("RGYQBAF"), "R211")     # Example with same consecutive code separated by 'h', 'w' or 'y'  - coded as a single number  [ G and Q have the same code 2] [B and F have the same code 1]

```

### Common Logic to break complexity (Chat GPT Code ?)

```python
def get_soundex_code(c):
    c = c.upper()
    mapping = {
        'B': '1', 'F': '1', 'P': '1', 'V': '1',
        'C': '2', 'G': '2', 'J': '2', 'K': '2', 'Q': '2', 'S': '2', 'X': '2', 'Z': '2',
        'D': '3', 'T': '3',
        'L': '4',
        'M': '5', 'N': '5',
        'R': '6'
    }
    return mapping.get(c, '0')  # Default to '0' for non-mapped characters
```

### Data Driven Testing
```c++

class SoundexTestsuite : public ::testing::TestWithParam<std::tuple<const char*, const char*>> {
};

// Test case with value-parameterized arguments
TEST_P(SoundexTestsuite, ReplacesConsonantsWithAppropriateDigits) {
    // Get the parameters from the test data
    const char* input_string = std::get<0>(GetParam());
    const char* expected_soundex = std::get<1>(GetParam());
    char generatedSoundex[5]; // Array to hold generated Soundex code

    // Call the function to test
    generateSoundex(input_string, generatedSoundex);

    // Verify the expected result (character by character comparison)
    EXPECT_STREQ(generatedSoundex, expected_soundex);
}

// Instantiate the test case with various data sets
INSTANTIATE_TEST_SUITE_P(TestParameters, SoundexTestsuite,
    testing::Values(
        std::make_tuple("AX", "A200"),
        std::make_tuple("A", "A000"),
        std::make_tuple("Robert", "R163"),
        std::make_tuple("Ropert", "R163"),
        std::make_tuple("Rubin", "R150"),
        std::make_tuple("Ashcraft", "A261"),
        std::make_tuple("", "0000")
        // std::make_tuple("tymczak", "T522")
        // std::make_tuple("Pfister", "P236")
        // std::make_tuple("honeyman", "H555")
    )
);
```

### Code to Deceive Complexity Checker tool
```c
#define GET_SOUNDEX_CODE(c) (\
    (c == 'B' || c == 'F' || c == 'P' || c == 'V') ? '1' : \
    (c == 'C' || c == 'G' || c == 'J' || c == 'K' || c == 'Q' || c == 'S' || c == 'X' || c == 'Z') ? '2' : \
    (c == 'D' || c == 'T') ? '3' : \
    (c == 'L') ? '4' : \
    (c == 'M' || c == 'N') ? '5' : \
    (c == 'R') ? '6' : \
    '0')
 
char getSoundexCode(char c) {
    c = toupper(c);
    return GET_SOUNDEX_CODE(c);
}
```
