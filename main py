from flask import Flask, request, send_file
from PIL import Image, ImageDraw, ImageFont
import requests
from io import BytesIO

app = Flask(__name__)

@app.route('/welcome-card', methods=['GET'])
def welcome_card():
    text1 = request.args.get('text1', '')
    text2 = request.args.get('text2', '')
    text3 = request.args.get('text3', '')
    avatar_url = request.args.get('avatar', '')

    # Crear una imagen en blanco
    width, height = 800, 400
    image = Image.new('RGB', (width, height), color=(73, 109, 137))

    draw = ImageDraw.Draw(image)

    # Cargar fuente
    try:
        font = ImageFont.truetype("arial.ttf", 40)
    except IOError:
        font = ImageFont.load_default()

    # Añadir textos a la imagen
    draw.text((width // 2, 50), text1, font=font, fill=(255, 255, 255), anchor="mm")
    draw.text((width // 2, 150), text2, font=font, fill=(255, 255, 255), anchor="mm")
    draw.text((width // 2, 250), text3, font=font, fill=(255, 255, 255), anchor="mm")

    # Descargar y añadir avatar
    if avatar_url:
        response = requests.get(avatar_url)
        avatar = Image.open(BytesIO(response.content))
        avatar = avatar.resize((100, 100))
        image.paste(avatar, (width // 2 - 50, 300))

    # Guardar la imagen en un archivo temporal
    temp_file = BytesIO()
    image.save(temp_file, 'PNG')
    temp_file.seek(0)

    return send_file(temp_file, mimetype='image/png')

if __name__ == '__main__':
    app.run(debug=True)
