#!/usr/bin/python3
# SZMELC CODE VISUALIZER V2 
# with Super Smooth Rounded Corners

import os
from PIL import Image, ImageDraw
from pygments import highlight
from pygments.lexers import guess_lexer, get_lexer_for_filename
from pygments.formatters import ImageFormatter
from io import BytesIO

def round_corners(image, radius, scale=10):
    # Scale up the radius for the high-res mask
    high_res_radius = radius * scale

    # Create a high-res mask to match the scaled image size
    high_res_size = (image.size[0] * scale, image.size[1] * scale)
    high_res_mask = Image.new("L", high_res_size, 0)
    draw = ImageDraw.Draw(high_res_mask)

    # Draw a high-res rounded rectangle on the mask
    full_rect = [0, 0, high_res_mask.size[0], high_res_mask.size[1]]
    draw.rounded_rectangle(full_rect, high_res_radius, fill=255)

    # Scale down the mask to the original image size
    try:
        # For Pillow versions 8.0.0 and later
        mask = high_res_mask.resize(image.size, Image.Resampling.LANCZOS)
    except AttributeError:
        # For older versions of Pillow
        mask = high_res_mask.resize(image.size, Image.ANTIALIAS)

    # Apply the smoothed mask to the image
    rounded_image = image.copy()
    rounded_image.putalpha(mask)

    return rounded_image

def create_image_with_code(code_file, corner_radius=10):
    # Read the code from file
    with open(code_file, 'r') as file:
        code = file.read()

    # Try to guess the lexer based on file content or use a default
    try:
        lexer = guess_lexer(code)
    except:
        lexer = get_lexer_for_filename(code_file, code)

    # Syntax highlighting
    formatter = ImageFormatter(font_name='DejaVu Sans Mono', style='monokai', line_numbers=False)
    code_image = highlight(code, lexer, formatter)

    # Convert to PIL image
    image_stream = BytesIO(code_image)
    image = Image.open(image_stream)

    # Round the corners with high smoothness
    rounded_image = round_corners(image, corner_radius)

    # Output file path
    output_image_path = os.path.splitext(code_file)[0] + '_rounded.png'

    # Save the image
    rounded_image.save(output_image_path)

# Ask user for the file path
file_path = input("Enter the path of the file to visualize: ")

# Usage
create_image_with_code(file_path)
