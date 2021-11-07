# Caesar_cipher
# -*- coding: utf-8 -*-
from string import ascii_letters

"""
Шифровка с поддержкой английского и русских языков, а также регистра.
"""


def encode_caesar_cipher(text, shift, base_shift, ord_up, ord_low):
    """ Шифр Цезаря: Зашифровка"""
    encode_shift = base_shift - shift
    cipher = ""
    for letter in text:
        if letter.isalpha():
            if letter.isupper():
                cipher += chr(ord(letter) - encode_shift + (ord(letter) - encode_shift < ord_up) * base_shift)
            else:
                cipher += chr(ord(alpha) - encode_shift + (ord(letter) - encode_shift < ord_low) * base_shift)
        else:
            cipher += letter
    return cipher


def decode_caesar_cipher(text, shift, base_shift, ord_up, ord_low):
    """ Шифр Цезаря: Расшифровка"""
    cipher = ""
    for letter in text:
        if letter.isalpha():
            if letter.isupper():
                cipher += chr(ord(letter) - shift + (ord(letter) - shift < ord_up) * base_shift)
            else:
                cipher += chr(ord(letter) - shift + (ord(letter) - shift < ord_low) * base_shift)
        else:
            cipher += letter
    return cipher


print("Добро пожаловать в Шифр Цезаря!")


def settings():
    print('Пожалуйста, выберите один из двух языков(eng, rus): ')
    while True:
        language = input(">>> ")
        if language == "eng":
            right_shift = 25
            incorrect_word = "русские"
            verify_alphas = "абвгдежзийклмнопрстуфхцчшщъыьэюяАБВГДЕЖЗИЙКЛМНОПРСТУФХЦЧШЩЪЫЬЭЮЯ"
            ord_upper = 65
            ord_lower = 97
            return language, right_shift, incorrect_word, verify_alphas, ord_upper, ord_lower
        elif language == "rus":
            right_shift = 31
            incorrect_word = "английские"
            verify_alphas = ascii_letters
            ord_upper = 1040
            ord_lower = 1072
            return language, right_shift, incorrect_word, verify_alphas, ord_upper, ord_lower


LANGUAGE, RIGHT_SHIFT, INCORRECT_WORD, VERIFY_ALPHAS, ORD_UPPER, ORD_LOWER = settings()
original_text = input("Введите искомый текст: ")
# Проверка текста на наличие другого языка
flag = True
while flag:
    for alpha in original_text:
        if alpha in VERIFY_ALPHAS:
            original_text = input(f"В тексте есть {INCORRECT_WORD} буквы. Попробуйте ещё раз: ")
            break
    else:
        print("Искомый текст успешно введён")
        flag = False

# Программа не работает с буквой 'Ё/ё'. Заменяем все совпадения на 'Е/е'.
original_text = original_text.replace("Ё", "Е").replace("ё", "е")

original_shift = input(f"Введите величину сдвига текста(от 1 до {RIGHT_SHIFT}): ")
# Проверка валидности введённых данных
while True:
    if not original_shift.isdigit():
        original_shift = input("Введённый сдвиг должен быть числом. Попробуйте ещё раз: ")
    if not 1 <= int(original_shift) <= 25:
        original_shift = input("Введённый сдвиг должен быть числом от 1 до 25. Попробуйте ещё раз: ")
    else:
        original_shift = int(original_shift)
        break

coding = input("Выберите опцию: Шифровка текста(encode) | Расшифровка текста(decode):")
while coding not in ["decode", "encode"]:
    coding = input(">>> ")

match coding:
    case "decode" | "encode" as code:
        result = code + "_caesar_cipher" + "(original_text, original_shift, (RIGHT_SHIFT + 1), ORD_UPPER, ORD_LOWER)"

print(eval(result))
