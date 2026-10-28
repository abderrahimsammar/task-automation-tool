# task-automation-tool
import os
import shutil

# دالة لأتمتة تنظيم الملفات
def organize_files(directory_path):
    # قاموس يحدد المجلد الذي ستذهب إليه كل صيغة ملف
    file_types = {
        'Images': ['.jpg', '.jpeg', '.png', '.gif', '.bmp'],
        'Documents': ['.pdf', '.docx', '.doc', '.txt', '.xlsx', '.pptx'],
        'Videos': ['.mp4', '.mkv', '.avi', '.mov'],
        'Music': ['.mp3', '.wav', '.aac'],
        'Archives': ['.zip', '.rar', '.7z'],
        'Scripts': ['.py', '.js', '.html', '.css', '.json']
    }

    # التأكد من أن المسار موجود
    if not os.path.exists(directory_path):
        print(f"المسار {directory_path} غير موجود.")
        return

    # المرور على كل الملفات في المجلد
    for filename in os.listdir(directory_path):
        file_path = os.path.join(directory_path, filename)

        # تجاهل المجلدات، نريد الملفات فقط
        if os.path.isfile(file_path):
            file_extension = os.path.splitext(filename)[1].lower()
            moved = False

            # البحث عن المجلد المناسب للملف
            for folder_name, extensions in file_types.items():
                if file_extension in extensions:
                    target_folder = os.path.join(directory_path, folder_name)
                    
                    # إنشاء المجلد إذا لم يكن موجوداً
                    if not os.path.exists(target_folder):
                        os.makedirs(target_folder)
                    
                    # نقل الملف
                    try:
                        shutil.move(file_path, os.path.join(target_folder, filename))
                        print(f"تم نقل: {filename} إلى {folder_name}")
                        moved = True
                    except Exception as e:
                        print(f"خطأ في نقل {filename}: {e}")
                    break
            
            # إذا لم يتم العثور على مجلد مناسب، نضعه في مجلد "Others"
            if not moved:
                others_folder = os.path.join(directory_path, "Others")
                if not os.path.exists(others_folder):
                    os.makedirs(others_folder)
                shutil.move(file_path, os.path.join(others_folder, filename))
                print(f"تم نقل: {filename} إلى Others")

if __name__ == "__main__":
    # ضع هنا مسار المجلد الذي تريد تنظيمه (مثلاً مجلد التنزيلات)
    # مثال للويندوز: "C:\\Users\\YourName\\Downloads"
    # مثال للمسك: "/Users/YourName/Downloads"
    target_directory = input("أدخل مسار المجلد الذي تريد تنظيمه: ")
    organize_files(target_directory)
    print("تمت عملية التنظيم بنجاح!")
