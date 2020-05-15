# Module_0 project - Random number guess game

import numpy

    
def game_core(number): #Функция для определения количества попыток, которое потребовалось для угадывания числа
    global predict_greater_list
    global predict_less_list
    predict_greater_list = []
    predict_less_list = []
    count = 1
    direction = ''
    predict = numpy.random.randint(1,101) #первый прогноз числа
    while number != predict:
        count+=1
        if count > 50:
            break
        elif number > predict:
            direction = 'more than predict' #переменная указывает на то, что искомое число больше прогноза
            predict = predict_change(predict, direction)
        elif number < predict: 
            direction = 'less than predict' #переменная указывает на то, что число меньше прогноза
            predict = predict_change(predict, direction)
    return(count) # выход из цикла, если угадали


def predict_change(predict, direction): #Функция для получения нового прогноза искомого числа
        if direction == 'more than predict':
            predict_less_list.append(predict) #список прогнозов меньше искомого числа
            if len(predict_greater_list) < 1: #если пуст список прогнозов превышающих искомое число
                predict = IfSpanTo100(predict, 100) 
            else:
                border_top = sorted(predict_greater_list)[0] #верхняя граница диапазона поиска прогноза
                predict = IfSpanTo100(predict, border_top)
        elif direction == 'less than predict':
            predict_greater_list.append(predict)
            if len(predict_less_list) < 1:
                predict = IfSpanTo1(predict, 1)
            else:
                border_bottom = sorted(predict_less_list)[-1] #нижняя граница диапазона поиска прогноза
                predict = IfSpanTo1(predict, border_bottom)
        return(predict)
    

def IfSpanTo100(predict, border_top): #Функция для расчета следующего прогноза в сторону увеличения
    span = len(range(predict, border_top)) #диапазон в котором расчитывается следующий прогоноз
    if span < 3:
        predict_new = predict + 1 #при коротком диапазоне перебор прогноза по одному значению
    else:
        predict_new = predict + int(span/2) #следующий прогноз расчитывается как середина диапазона
    return predict_new #новый прогноз
 
    
def IfSpanTo1(predict, border_bottom): #Функция для расчета прогноза в сторону уменьшения
    span = len(range(border_bottom, predict))
    if span < 3:
        predict_new = predict - 1
    else:
        predict_new = predict - int(span/2)
    return predict_new

	
def score_game(game_core): #Функция 1000 раз обращается к функции game_core чтобы получить среднее количество попыток угадывания
    count_ls = []
    numpy.random.seed(1)  # фиксируем RANDOM SEED, чтобы ваш эксперимент был воспроизводим!
    random_array = numpy.random.randint(1,101, size=(1000))
    for number in random_array:
        count_ls.append(game_core(number)) #список количества попыток
    score = int(sum(count_ls)/len(count_ls)) #среднее арифметическое из количества попыток
    print(f"Ваш алгоритм угадывает число в среднем за {score} попыток")
    return(score)

score_game(game_core)