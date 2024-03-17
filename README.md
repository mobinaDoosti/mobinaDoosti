from fastapi import FastAPI, HTTPException

app = FastAPI()

def validate_student_number(student_number):
    if len(student_number) != 11:
        return False, "شماره دانشجویی باید 11 رقم باشد"
    
    year = student_number[:3]
    fixed_part = student_number[3:9]
    index = student_number[9:]

    if not (400 <= int(year) <= 402):
        return False, "قسمت سال نادرست است"
    
    if fixed_part != "114150":
        return False, "قسمت ثابت نادرست است"
    
    if not (1 <= int(index) <= 99):
        return False, "قسمت اندیس نادرست است"
    
    return True, "شماره دانشجویی درست است"

@app.get("/student/{student_number}")
async def get_student_number(student_number: str):
    is_valid, message = validate_student_number(student_number)
    if not is_valid:
        raise HTTPException(status_code=400, detail=message)
    return {"student_number": student_number}
