# jap_translation
# I am a university student and trying to create translation file from Japanese to English, or vice versa.
# The Japanese term "日本語” means "Japanese", for other terms can be ignored.

"""Let's play 翻訳ゲーム!"""

import pykakasi

kakasi = pykakasi.kakasi()
kakasi.setMode("J", "H")
converter = kakasi.getConverter()


def get_translations_from_file(filename):
    """translate English to Japanese, Japanese to English from file"""
    infile = open(filename,"r", encoding="utf-8")
    contents = infile.read()
    translations = get_translations(contents)
    infile.close()
    return translations
    
def get_translations(string):
    """translate English to Japanese, Japanese to English"""
    lines = string.splitlines()
    header = lines[0]
    translations = []
    if header != "日本語,english" and header != "english,日本語":
        print("Header language not recognized!")
    for line in lines[1:]:
        words = line.split(",")
        if header == "english,日本語":
            translations.append((words[0], words[1]))
        elif header == "日本語,english":
            translations.append((words[1], words[0]))
    return translations
    
def jap_test(translations):
    """Let's play a translating game"""
    if not translations:
        print("We can't play, 残念!")
        return -1
    incorrect_answers = translations[:]
    print(f"こんにちわ, you have {len(translations)} terms to translate today :-)")
    count = 0
    while incorrect_answers:
        next_round = []
        for english_term, 日本語_term in incorrect_answers:
            print(f"en: {english_term}")
            student_answer = input("mi: ")
            if converter.do(student_answer) == converter.do(日本語_term):
                print("正解！")
            else:
                print(f'A: {answer_mask(日本語_term, student_answer)}')
                next_round.append((english_term, 日本語_term))
            count += 1
        incorrect_answers = next_round
    correct = (len(translations) / count) * 100
    return correct

def answer_mask(日本語_term, student_answer):
    """produce a string the same length as the Japanese string"""
    result = ""
    for i in range(len(日本語_term)):
        if i < len(student_answer) and converter.do(日本語_term[i]) == converter.do(student_answer[i]):
            result += "*"
        else:
            result += 日本語_term[i]
    return result

def main():
    """Let's play 翻訳ゲーム"""
    filename = input('Please enter a filename: ')
    contents = get_translations_from_file(filename)
    correct = jap_test(contents)
    if correct != -1:
        print("You translated all the terms!")
        print(f"You scored {correct:.2f}%")
    else:
        print("Bye!")

main()


