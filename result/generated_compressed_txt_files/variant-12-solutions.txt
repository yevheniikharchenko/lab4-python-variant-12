def get_flight_notification(h:float, v_y:float) -> str:
    match (h, v_y):
        case (h, v_y) if h < 300.0 and v_y > 15.0:
            return "PULL UP! (Негайний набір висоти)"
        case (h, v_y) if h < 100.0:
            return "TOO LOW! (Низька висота)"
        case _:
            return "Параметри безпечні"

def id_flight_state(state:dict) -> str:
    match state:
        case {"power" : p, "thrust" : t, "mode" : m} if m == "BOOST" and p > 5000:
            return "Currently boosting"
        case {"power" : p, "thrust" : t, "mode" : m} if m == "ECO":
            return "Currently eco"
        case {"power" : p, "thrust" : t, "mode" : m}:
            return f"Unid-ed state: {m}"
        case _:
            return "Invalid input"

def RAM_check(percentages: list) -> bool:
    for p in percentages:
        if p > 0.95:
            print("Скидання буфера на Flash-пам'ять")
            return True
    return False

def get_TPU_rising(T : list, P : list) -> list:
    match T,P:
        case [*_],[*_] if len(T) == len(P):
            result = []
            for index, (t, p) in enumerate(zip(T, P)):
                if (t > 0) and (p > 0):
                    result.append(index)
            return result
        case [*_],[*_]:
            print("Списки похідних мають різну довжину!")
            return []
        case _:
            print("Некоректні дані!")
            return []
