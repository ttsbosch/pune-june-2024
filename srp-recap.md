### What would be the better name?
```js
function printManual() {
    const minorSize = MajorColorNames.length;
    const majorSize = MinorColorNames.length;
    let manual = '';

    for (let i = 1; i <= minorSize * majorSize; i++) {
        const colorPair = getColorFromPairNumber(i);
        manual += `Pair Number: ${i}, Colors: ${colorPair.toString()}\n`;
    }

    return manual;
}
```

### YAGNI Violation ,Testable ?
```c
void PrintColorCodingReference() {
    int pairNumber = 1;

    for (int major = 0; major < numberOfMajorColors; major++) {
        for (int minor = 0; minor < numberOfMinorColors; minor++) {
            printf("%d. %s-%s\n", pairNumber, MajorColorNames[major], MinorColorNames[minor]);
            pairNumber++;
        }
    }
}

```py
def display_color():
   print("\n================== Color Coding Reference Manual =====================")
   for pair_number in range(1,26):
       major_color, minor_color = get_color_from_pair_number(pair_number)
       print(pair_number,color_pair_to_string(major_color, minor_color))

```

```c
void printColorReferenceManual()
{
    printf("Color Code Reference Manual:\n");
    printf("-----------------------------\n");
    for (int i = 1; i <= numberOfMajorColors * numberOfMinorColors; ++i) 
    {
        ColorPair colorPair = GetColorFromPairNumber(i);
        char colorPairNames[MAX_COLORPAIR_NAME_CHARS];
        ColorPairToString(&colorPair, colorPairNames);
        printf("%2d: %s\n", i, colorPairNames);
    }
    printf("-----------------------------\n");
}
```
``` Java

 public static void generateColorCodeManual(){
        MajorColor.Color[] majorColors = MajorColor.Color.values();
        MinorColor.Color[] minorColors = MinorColor.Color.values();

        String referenceManual = ColorUtil.generateReferenceManual(majorColors, minorColors);
        System.out.println(referenceManual);
    }
```
### FAT Interface
- https://github.com/tts-tcq-2024/modular-in-c-vasi091.git
- https://github.com/tts-tcq-2024/modular-in-cpp-Akshara-sandha.git

### DEO
- https://github.com/tts-tcq-2024/modular-in-c-Shankar0920.git
- https://github.com/tts-tcq-2024/modular-in-c-Shanmukharao9.git
- https://github.com/tts-tcq-2024/modular-in-c-Saipavan-Poosala.git
- https://github.com/tts-tcq-2024/modular-in-cpp-sumanthsags.git
- https://github.com/tts-tcq-2024/modular-in-py-Ganasai549.git
- 
#### When Name Reflects Requirement
```py
def create_reference_manual():
    manual = []
    for major in MAJOR_COLORS:
        for minor in MINOR_COLORS:
            pair_number = get_pair_number_from_color(major, minor)
            manual.append(f'{pair_number:2d} - {color_pair_to_string(major, minor)}')
    return '\n'.join(manual)

```

```c++
std::map<std::uint8_t, ColorPair> TeleComm::getColorPairList()
{
  std::map<std::uint8_t, ColorPair> colorPairList;
  for(std::size_t pairNo = 1; pairNo <=  maxColorPair; pairNo++)
   {
      colorPairList.insert({pairNo, getColorFromPairNumber(static_cast<int>(pairNo))});
   }
   return colorPairList;
}
```

```py
def build_color_guide():
    manual = "Color Coding Reference:\n"
    for pair_number in range(1, len(MAJOR_COLORS) * len(MINOR_COLORS) + 1):
        major_color, minor_color = get_color_from_pair_number(pair_number)
        manual += f"Pair Number {pair_number}: {major_color} {minor_color}\n"
    return manual
```
```
