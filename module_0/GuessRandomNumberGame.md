# Module_0 project - Random number guess game

import numpy
predict_list = []


def game_core(number): #Функция для определения количества попыток, которое потребовалось для угадывания числа
    count = 1
    direction = ''
    predict = numpy.random.randint(1,101)
    while number != predict:
        count+=1
        if count > 50:
            break
        elif number > predict:
            direction = 'more than predict' #переменная указывает на то, что искомое число больше прогноза
            predict_list.append(predict) #в списке фиксируются неудачные прогнозы числа
            predict = predict_change(direction, count)
        elif number < predict: 
            direction = 'less than predict' #переменная указывает на то, что число меньше прогноза
            predict_list.append(predict)
            predict = predict_change(direction, count)
        return(count) # выход из цикла, если угадали


def predict_change(direction, count): #Функция для получения нового прогноза искомого числа
        if direction == 'more than predict' and count < 4:  #пока в predict_list меньше 3 значений расчет прогноза для указанного direction делается в этой ветке
        span = len(range(predict_list[-1], 100))
        predict = predict_list[-1]+int(span/2)  #следующий прогноз числа расчитывается как середина отрезка span
        elif direction == 'less than predict'and count < 4:
            span = len(range(1, predict_list[-1]))+1
            predict = predict_list[-1]-int(span/2) 
        elif direction == 'more than predict':
            if predict_list[-1] < predict_list[-2]:
                span = len(range(predict_list[-1], predict_list[-2]))
                if span < 3: #если отрезок короче 3 значений, то прогноз подбирается с шагом на единицу
                    predict = predict_list[-1] + 1
                else:
                    predict = predict_list[-1]+int(span/2)
            elif predict_list[-1] > predict_list[-2]:
                if predict_list[-3] < predict_list[-1]:
                    span = len(range(predict_list[-1], 100))
                    if span < 3:
                        predict = predict_list[-1] + 1
                    else:
                        predict = predict_list[-1]+int(span/2)
            else:
                span = len(range(predict_list[-1], predict_list[-3]))
                if span < 3:
                    predict = predict_list[-1] + 1
                else:
                    predict = predict_list[-1]+int(span/2)
        elif direction == 'less than predict':
            if predict_list[-1] < predict_list[-2]:
                if predict_list[-3] > predict_list[-1]:
                    span = len(range(1, predict_list[-1]))
                    if span < 3:
                        predict = predict_list[-1] - 1
                    else:
                        predict = predict_list[-1]-int(span/2)
                else: 
                    span = len(range(predict_list[-3], predict_list[-1]))
                    if span < 3:
                        predict = predict_list[-1] - 1
                    else:
                        predict = predict_list[-1]-int(span/2)
            elif predict_list[-1] > predict_list[-2]:
                span = len(range(predict_list[-2], predict_list[-1]))
                if span < 3:
                    predict = predict_list[-1] - 1
                else:
                    predict = predict_list[-1]-int(span/2)   
    return(predict)
 
 
def score_game(game_core): #Функция 1000 раз обращается к функции game_core чтобы получить среднее количество попыток угадывания
    count_ls = []
    numpy.random.seed(1)  # фиксируем RANDOM SEED, чтобы ваш эксперимент был воспроизводим!
    random_array = numpy.random.randint(1,101, size=(1000))
    for number in random_array:
        count_ls.append(game_core(number))
    score = int(sum(count_ls)/len(count_ls))
    print(f"Ваш алгоритм угадывает число в среднем за {score} попыток")
    return(score)

score_game(game_core)