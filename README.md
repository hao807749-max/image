import os
import random
from PIL import Image, ImageDraw, ImageFont

# 输出文件夹
OUTPUT_DIR = "output_images"
os.makedirs(OUTPUT_DIR, exist_ok=True)

# 可选字体列表（根据系统可用字体调整）
FONTS = ["arial.ttf", "times.ttf", "calibri.ttf"]

def random_color():
    """生成随机RGB颜色"""
    return tuple(random.randint(0, 255) for _ in range(3))

def draw_random_shapes(draw, width, height, num_shapes=5):
    """绘制随机几何图形"""
    for _ in range(num_shapes):
        shape_type = random.choice(["rectangle", "ellipse", "line"])
        x0, y0 = random.randint(0, width//2), random.randint(0, height//2)
        x1, y1 = random.randint(width//2, width), random.randint(height//2, height)
        color = random_color()
        if shape_type == "rectangle":
            draw.rectangle([x0, y0, x1, y1], outline=color, width=3)
        elif shape_type == "ellipse":
            draw.ellipse([x0, y0, x1, y1], outline=color, width=3)
        elif shape_type == "line":
            draw.line([x0, y0, x1, y1], fill=color, width=3)

def create_random_image(filename):
    width, height = 512, 512
    img = Image.new("RGB", (width, height), color=random_color())
    draw = ImageDraw.Draw(img)

    # 绘制随机几何图形
    draw_random_shapes(draw, width, height)

    # 添加随机文字
    try:
        font_path = random.choice(FONTS)
        font_size = random.randint(24, 60)
        font = ImageFont.truetype(font_path, font_size)
    except:
        font = ImageFont.load_default()

    text = "Hello AI!"
    text_color = random_color()
    text_width, text_height = draw.textsize(text, font=font)
    x = (width - text_width) // 2
    y = (height - text_height) // 2
    draw.text((x, y), text, fill=text_color, font=font)

    img.save(os.path.join(OUTPUT_DIR, filename))
    print(f"已生成图片: {filename}")

if __name__ == "__main__":
    # 生成 5 张图片
    for i in range(5):
        create_random_image(f"random_image_{i+1}.png")
