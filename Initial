import pygame
import sys
import os
import math
import random
import time

# Initialize Pygame
pygame.init()

# Screen dimensions - wider screen
WIDTH, HEIGHT = 1400, 700
screen = pygame.display.set_mode((WIDTH, HEIGHT))
pygame.display.set_caption("Forest Defenders: Magical Numbers Adventure")
clock = pygame.time.Clock()
font = pygame.font.SysFont('Arial', 30)
small_font = pygame.font.SysFont('Arial', 24)
dialog_font = pygame.font.SysFont('Arial', 22)

# Character PNGs directory
image_path = r"C:\Users\Mykho\Downloads"

# Character image names (no extension)
character_names = [
    "Character1", "Character2", "Character3", "Character4",
    "Character5", "Character6", "Character7", "Character8"
]

# Add this code after your initial imports
import time

# Create procedural animated cavern background
def create_animated_cavern_background():
    # Base surface for the cavern
    base_surface = pygame.Surface((WIDTH, HEIGHT))
    base_surface.fill((30, 40, 80))  # Dark blue base
    
    # Create crystal formations that will be animated
    crystal_formations = []
    for _ in range(30):
        x = random.randint(0, WIDTH)
        y = random.randint(0, HEIGHT)
        size = random.randint(15, 40)
        color_type = random.choice(["pink", "blue"])
        if color_type == "pink":
            color = (random.randint(200, 255), random.randint(50, 150), random.randint(180, 255))
        else:
            color = (random.randint(50, 150), random.randint(180, 255), random.randint(200, 255))
            
        # Each crystal has its own pulse rate and offset
        pulse_rate = random.uniform(0.5, 2.0)
        time_offset = random.uniform(0, 3.14)  # Random phase offset
        
        crystal_formations.append({
            "x": x,
            "y": y,
            "size": size,
            "color": color,
            "pulse_rate": pulse_rate,
            "time_offset": time_offset
        })
    
    # Add some static elements to the base
    for _ in range(20):
        x = random.randint(0, WIDTH)
        y = random.randint(0, HEIGHT)
        size = random.randint(10, 30)
        # Darker crystals for the base
        color = (
            random.randint(20, 70),
            random.randint(30, 80),
            random.randint(60, 120)
        )
        pygame.draw.polygon(base_surface, color, [
            (x, y - size),
            (x + size//2, y + size),
            (x - size//2, y + size)
        ])
    
    return base_surface, crystal_formations

# Replace the cavern background loading with this:
try:
    # First try to load the file
    cavern_path = os.path.join(image_path, "Cavern.gif")
    if os.path.exists(cavern_path):
        print(f"Loading Cavern.gif from: {cavern_path}")
        # For static display of the first frame
        cavern_background = pygame.image.load(cavern_path).convert()
        cavern_background = pygame.transform.scale(cavern_background, (WIDTH, HEIGHT))
        # Flag to indicate we're using a static image
        using_procedural_cavern = False
    else:
        print("Cavern.gif not found, creating procedural animated cavern")
        cavern_base, cavern_crystals = create_animated_cavern_background()
        cavern_background = cavern_base  # Initial background
        using_procedural_cavern = True
except Exception as e:
    print(f"Cavern background error: {e}")
    cavern_base, cavern_crystals = create_animated_cavern_background()
    cavern_background = cavern_base
    using_procedural_cavern = True

# Add this function to update the animated cavern
def update_cavern_background():
    if not using_procedural_cavern:
        return cavern_background
        
    # Create a copy of the base surface
    current_frame = cavern_base.copy()
    
    # Current time for animations
    current_time = time.time()
    
    # Draw animated crystals
    for crystal in cavern_crystals:
        # Calculate pulsing effect based on time
        pulse = math.sin(current_time * crystal["pulse_rate"] + crystal["time_offset"])
        pulse_value = abs(pulse)  # 0 to 1 pulse value
        
        # Size varies slightly with pulse
        current_size = int(crystal["size"] * (0.9 + 0.2 * pulse_value))
        
        # Color also pulses slightly
        r, g, b = crystal["color"]
        pulse_intensity = int(70 * pulse_value)
        current_color = (
            min(255, r + pulse_intensity),
            min(255, g + pulse_intensity),
            min(255, b + pulse_intensity)
        )
        
        # Draw the crystal
        pygame.draw.polygon(current_frame, current_color, [
            (crystal["x"], crystal["y"] - current_size),
            (crystal["x"] + current_size//2, crystal["y"] + current_size//2),
            (crystal["x"] - current_size//2, crystal["y"] + current_size//2)
        ])
        
        # Add a glow effect
        glow_surface = pygame.Surface((current_size*3, current_size*3), pygame.SRCALPHA)
        glow_radius = current_size + int(current_size * 0.5 * pulse_value)
        glow_color = (current_color[0], current_color[1], current_color[2], 100)
        pygame.draw.circle(glow_surface, glow_color, (glow_surface.get_width()//2, glow_surface.get_height()//2), glow_radius)
        current_frame.blit(glow_surface, (crystal["x"] - glow_surface.get_width()//2, crystal["y"] - glow_surface.get_height()//2), special_flags=pygame.BLEND_ADD)
    
    return current_frame

# --- In the main game loop ---
# Add this code right after "dt = clock.tick(60)"
    # Update the cavern background if we're in stage 2
    if game_stage == 2 and using_procedural_cavern:
        background = update_cavern_background()
    elif game_stage == 2:
        background = cavern_background
    elif game_stage == 1:
        background = gameplay_background
    elif game_stage == 0:
        background = selection_background

# Load backgrounds
try:
    # Selection screen background
    selection_background = pygame.image.load(os.path.join(image_path, "Background.jpg")).convert()
    selection_background = pygame.transform.scale(selection_background, (WIDTH, HEIGHT))
    
    # Gameplay background - forest (Stage 1)
    gameplay_background = pygame.image.load(os.path.join(image_path, "Background0.png")).convert()
    gameplay_background = pygame.transform.scale(gameplay_background, (WIDTH, HEIGHT))
    
    # Cavern background (Stage 2)
    try:
        # Try both extensions since you mentioned it might be a GIF
        if os.path.exists(os.path.join(image_path, "Cavern.png")):
            cavern_background = pygame.image.load(os.path.join(image_path, "Cavern.png")).convert()
        elif os.path.exists(os.path.join(image_path, "Cavern.gif")):
            cavern_background = pygame.image.load(os.path.join(image_path, "Cavern.gif")).convert()
        else:
            print("Cavern background not found with either .png or .gif extension")
            cavern_background = pygame.Surface((WIDTH, HEIGHT))
            cavern_background.fill((50, 80, 130))  # Dark blue-ish color for cavern
            
        cavern_background = pygame.transform.scale(cavern_background, (WIDTH, HEIGHT))
    except Exception as e:
        print(f"Cavern background error: {e}")
        cavern_background = pygame.Surface((WIDTH, HEIGHT))
        cavern_background.fill((50, 80, 130))  # Dark blue-ish color for cavern
except Exception as e:
    print(f"Background image not found, using fallback color: {e}")
    selection_background = pygame.Surface((WIDTH, HEIGHT))
    selection_background.fill((100, 150, 200))  # Fallback color for selection
    gameplay_background = pygame.Surface((WIDTH, HEIGHT))
    gameplay_background.fill((120, 170, 220))  # Slightly different fallback for gameplay
    cavern_background = pygame.Surface((WIDTH, HEIGHT))
    cavern_background.fill((50, 80, 130))  # Dark blue-ish color for cavern

# Current background
background = selection_background

# Load and scale character images - smaller size
CHARACTER_HEIGHT = 150  # Reduced from 200
characters = []

for name in character_names:
    file_path = os.path.join(image_path, name + ".png")
    if os.path.exists(file_path):
        img = pygame.image.load(file_path).convert_alpha()
        ow, oh = img.get_size()
        scale = CHARACTER_HEIGHT / oh
        img = pygame.transform.scale(img, (int(ow * scale), CHARACTER_HEIGHT))
        characters.append({"img": img, "name": name})
    else:
        print(f"Missing: {file_path}")

# If no characters found, create placeholder characters
if not characters:
    for i, name in enumerate(character_names):
        # Create a simple character silhouette as placeholder
        char_surface = pygame.Surface((75, CHARACTER_HEIGHT), pygame.SRCALPHA)
        color = (100 + i*20, 150, 255 - i*20)  # Varied colors without random
        pygame.draw.ellipse(char_surface, color, (0, 0, 75, 135))
        pygame.draw.circle(char_surface, color, (37, 30), 30)
        characters.append({"img": char_surface, "name": name})

# Create NPC
def create_npc():
    npc_surface = pygame.Surface((80, 160), pygame.SRCALPHA)
    # NPC body (green wizard)
    pygame.draw.ellipse(npc_surface, (20, 150, 50), (0, 40, 80, 120))
    # NPC head
    pygame.draw.circle(npc_surface, (220, 180, 130), (40, 40), 30)
    # NPC hat
    pygame.draw.polygon(npc_surface, (50, 100, 200), [(10, 40), (40, 0), (70, 40)])
    # NPC staff
    pygame.draw.line(npc_surface, (139, 69, 19), (75, 40), (75, 150), 5)
    return npc_surface

npc_img = create_npc()
npc_rect = npc_img.get_rect(bottomleft=(WIDTH - 200, HEIGHT - 200))

# Create Cavern NPC for Stage 2
def create_cavern_npc():
    npc_surface = pygame.Surface((80, 160), pygame.SRCALPHA)
    # NPC body (purple crystal sage)
    pygame.draw.ellipse(npc_surface, (100, 50, 150), (0, 40, 80, 120))
    # NPC head
    pygame.draw.circle(npc_surface, (200, 180, 220), (40, 40), 30)
    # NPC crystal crown
    pygame.draw.polygon(npc_surface, (180, 100, 255), [(20, 30), (40, 0), (60, 30)])
    pygame.draw.polygon(npc_surface, (180, 100, 255), [(10, 40), (20, 10), (30, 40)])
    pygame.draw.polygon(npc_surface, (180, 100, 255), [(50, 40), (60, 10), (70, 40)])
    # NPC crystal staff
    pygame.draw.line(npc_surface, (180, 100, 255), (75, 40), (75, 150), 5)
    pygame.draw.polygon(npc_surface, (200, 120, 255), [(65, 40), (75, 25), (85, 40)])
    return npc_surface

cavern_npc_img = create_cavern_npc()
cavern_npc_rect = cavern_npc_img.get_rect(bottomleft=(WIDTH - 200, HEIGHT - 200))

# Challenges for Stage 1 (disguised addition and subtraction)
stage1_questions = [
    {"question": "If the guardian collects 28 magical crystals and uses 13 for a spell, how many remain?", "answer": 15, "options": [12, 15, 16, 14]},
    {"question": "The guardian needs 45 energy points to create a forcefield. With 27 already gathered, how many more must they find?", "answer": 18, "options": [17, 18, 19, 20]},
    {"question": "Shadow creatures appear every 12 minutes. After 42 minutes, how many times have they visited our realm?", "answer": 3, "options": [3, 4, 5, 6]},
    {"question": "A magical potion requires 24 drops of moonwater and 9 drops of sunlight essence. How many drops in total?", "answer": 33, "options": [31, 32, 33, 34]},
    {"question": "Each protective enchantment lasts 17 minutes. To protect the forest for 85 minutes, how many enchantments must be cast?", "answer": 5, "options": [4, 5, 6, 7]}
]

# Challenges for Stage 2 (multiplication)
stage2_questions = [
    {"question": "The crystal sage needs to multiply 7 crystals by their power level of 8. What's the total energy?", "answer": 56, "options": [54, 55, 56, 57]},
    {"question": "Six magical cave formations each contain 9 power nodes. How many nodes total?", "answer": 54, "options": [52, 53, 54, 55]},
    {"question": "The cavern enchantment requires 7 groups of 12 crystal shards. How many shards in total?", "answer": 84, "options": [82, 83, 84, 85]},
    {"question": "Each of the 11 ancient pillars needs 6 energy cores. How many cores are needed altogether?", "answer": 66, "options": [64, 65, 66, 67]},
    {"question": "There are 9 crystal clusters, each with 8 crystals. How many crystals in all?", "answer": 72, "options": [70, 71, 72, 73]}
]

# Game state
current_character_index = 0
selection_mode = True
player_img = None
player_rect = None
facing = "right"
speed = 5
jump_velocity = 0
jump_strength = -15
gravity = 0.8
on_ground = True
bounce_timer = 0
game_stage = 0  # 0: intro, 1: forest, 2: cavern
dialog_active = False
current_dialog = []
dialog_index = 0
current_question = None
question_answered = False
questions_completed = 0
show_intro = False
intro_shown = False
stage_complete = False
answer_feedback = ""
feedback_timer = 0
showing_feedback = False
transition_to_stage2 = False
transition_timer = 0
switching_stage = False

# Stage 2 specific variable to track if intro has been shown
stage2_intro_shown = False

# Ground level (for jumping mechanics)
GROUND_LEVEL = HEIGHT - 210  # Adjusted based on background

# Arrow buttons for navigation - adjusted for wider screen
left_arrow = pygame.Rect(100, HEIGHT // 2, 50, 50)
right_arrow = pygame.Rect(WIDTH - 150, HEIGHT // 2, 50, 50)

# Game dialogs
intro_dialog = [
    "Welcome to the Enchanted Forest, brave adventurer!",
    "Our peaceful realm is under threat by the Shadow King, who has stolen the five Crystal Gems that maintain balance.",
    "Without these gems, the magical numbers and ancient symbols of our world are falling into chaos!",
    "You must journey through five realms to recover the crystal gems, using your magical reasoning skills to overcome challenges.",
    "In each realm, you'll need to solve mystical puzzles to gather energy for the final confrontation.",
    "This first area is the Whispering Woods, where the Forest Guardian awaits your help.",
    "Reach the Forest Guardian on the right side of the screen to begin your quest.",
    "Use arrow keys or WASD to move, and SPACE to jump. Good luck, young hero!"
]

npc_greeting = [
    "Greetings, young hero! I am the Guardian of the Whispering Woods.",
    "The Shadow King's minions have disrupted our forest's magical balance.",
    "I need your help with mystical calculations to restore our protective enchantments.",
    "Solve my magical challenges, and I'll grant you the power to proceed to the next realm.",
    "Are you ready to begin your first challenge? (Press SPACE to continue)"
]

npc_next_question = [
    "Excellent work, young wizard! But our forest still needs protection.",
    "Here's another puzzle that needs your magical reasoning powers.",
    "Focus your mind and trust your instincts! (Press SPACE to continue)"
]

npc_success = [
    "Remarkable! Your magical reasoning skills are truly impressive!",
    "You've helped restore balance to the Whispering Woods.",
    "I grant you this fragment of the Crystal Gem of Harmony.",
    "Ahead lies the Mystic Caverns, where new challenges await.",
    "The Cavern Guardian will test your powers of multiplication.",
    "Be brave, and remember: magical numbers are your allies!",
    "This fragment will transport you to the Caverns when you're ready.",
    "(Press SPACE to continue to the Mystic Caverns - Stage 2)"
]

stage2_intro = [
    "You've entered the Mystic Caverns, home to ancient crystals of great power.",
    "The shimmering crystals cast an ethereal glow throughout the cavern.",
    "In this sacred place, you must harness the power of multiplication to restore balance.",
    "Find the Crystal Sage deeper in the cavern to begin your challenges.",
    "(Use arrow keys or WASD to move, and SPACE to jump)"
]

cavern_npc_greeting = [
    "Ah, a visitor from the surface! I am the Crystal Sage of the Mystic Caverns.",
    "The Shadow King's corruption has reached even these depths, disrupting our crystal formations.",
    "We must use the power of multiplication to realign the magical energy flows.",
    "Are you ready to test your multiplication skills? The cavern's fate depends on it!",
    "(Press SPACE to continue)"
]

cavern_npc_next_question = [
    "Well done! Your magical arithmetic skills are impressive.",
    "But there are more crystal formations that need realignment.",
    "Here's another mystical calculation for you to solve.",
    "(Press SPACE to continue)"
]

cavern_npc_success = [
    "Magnificent! You've successfully realigned all the crystal formations!",
    "The Mystic Caverns once again resonate with harmonious energy.",
    "Take this fragment of the Crystal Gem of Multiplication as a token of our gratitude.",
    "Your next challenge awaits in another realm.",
    "(This completes the demo - Press SPACE to return to character selection)"
]

def select_character(index):
    global player_img, player_rect, facing, bounce_timer, background, game_stage, show_intro
    player_img = characters[index]["img"]
    player_rect = player_img.get_rect()
    player_rect.bottom = GROUND_LEVEL
    player_rect.x = 100  # Start on the left side
    facing = "right"
    bounce_timer = 0
    # Change background when entering gameplay mode
    background = gameplay_background
    game_stage = 1  # Start at stage 1 (forest)
    show_intro = True

def return_to_selection():
    global selection_mode, background, game_stage, dialog_active, current_dialog, dialog_index
    global show_intro, intro_shown, stage_complete, questions_completed, showing_feedback
    global stage2_intro_shown
    selection_mode = True
    # Switch back to selection background
    background = selection_background
    game_stage = 0
    dialog_active = False
    current_dialog = []
    dialog_index = 0
    show_intro = False
    intro_shown = False
    stage_complete = False
    questions_completed = 0
    showing_feedback = False
    stage2_intro_shown = False

def transition_to_cavern():
    # Simple transition without fade effect
    global game_stage, background, player_rect, stage_complete, questions_completed
    global dialog_active, current_dialog, transition_to_stage2, switching_stage
    global stage2_intro_shown
    
    # Update game state for stage 2
    game_stage = 2
    background = cavern_background
    player_rect.x = 100  # Reset player position
    player_rect.bottom = GROUND_LEVEL
    
    # Reset stage variables
    stage_complete = False
    questions_completed = 0
    switching_stage = False
    stage2_intro_shown = False
    
    # Show stage 2 intro dialog
    show_dialog(stage2_intro)
    transition_to_stage2 = False

def show_dialog(dialog_lines):
    global dialog_active, current_dialog, dialog_index
    dialog_active = True
    current_dialog = dialog_lines
    dialog_index = 0

def next_dialog():
    global dialog_index, dialog_active, intro_shown, show_intro, current_question
    global transition_to_stage2, switching_stage, stage2_intro_shown
    
    if dialog_index < len(current_dialog) - 1:
        dialog_index += 1
    else:
        dialog_active = False
        if current_dialog == intro_dialog:
            intro_shown = True
            show_intro = False
        elif current_dialog == stage2_intro:
            stage2_intro_shown = True
        elif current_dialog == npc_greeting or current_dialog == npc_next_question:
            # After closing greeting or next question dialog, show the question
            if not current_question and game_stage == 1:
                start_question(stage1_questions)
        elif current_dialog == cavern_npc_greeting or current_dialog == cavern_npc_next_question:
            # After closing cavern NPC dialog, show the question
            if not current_question and game_stage == 2:
                start_question(stage2_questions)
        elif current_dialog == npc_success:
            # Transition to stage 2 after success dialog
            transition_to_stage2 = True
            switching_stage = True
            # Transition immediately without timer
            transition_to_cavern()
        elif current_dialog == cavern_npc_success:
            # Return to character selection after completing stage 2
            return_to_selection()

def start_question(question_set):
    global current_question, question_answered
    if questions_completed < len(question_set):
        current_question = question_set[questions_completed]
        random.shuffle(current_question["options"])
        question_answered = False

def check_answer(selected_option):
    global question_answered, questions_completed, stage_complete, answer_feedback
    global showing_feedback, feedback_timer
    
    if selected_option == current_question["answer"]:
        answer_feedback = "Correct! Your magical reasoning is strong!"
        showing_feedback = True
        feedback_timer = 180  # 3 seconds at 60 FPS
        question_answered = True
        questions_completed += 1
        
        if questions_completed >= len(stage1_questions) and game_stage == 1:
            stage_complete = True
            # Will show success dialog after feedback timer expires
        elif questions_completed >= len(stage2_questions) and game_stage == 2:
            stage_complete = True
            # Will show stage 2 success dialog after feedback timer expires
        else:
            # Will show next question dialog after feedback timer expires
            pass
    else:
        answer_feedback = "Not quite right. Try a different magical approach!"
        showing_feedback = True
        feedback_timer = 180  # 3 seconds at 60 FPS

def draw_dialog_box():
    # Draw semi-transparent dialog box
    dialog_box = pygame.Surface((WIDTH - 200, 150), pygame.SRCALPHA)
    dialog_box.fill((0, 0, 0, 200))
    screen.blit(dialog_box, (100, HEIGHT - 200))
    
    # Draw text
    if dialog_index < len(current_dialog):
        text = dialog_font.render(current_dialog[dialog_index], True, (255, 255, 255))
        screen.blit(text, (120, HEIGHT - 170))
    
    # Draw continue prompt
    if dialog_index < len(current_dialog) - 1:
        continue_text = small_font.render("Press SPACE to continue...", True, (200, 200, 200))
    else:
        continue_text = small_font.render("Press SPACE to close", True, (200, 200, 200))
    screen.blit(continue_text, (WIDTH - 350, HEIGHT - 80))

def draw_question_box():
    # Draw question box
    question_box = pygame.Surface((WIDTH - 200, 300), pygame.SRCALPHA)
    question_box.fill((0, 0, 0, 200))
    screen.blit(question_box, (100, HEIGHT - 350))
    
    # Draw question text
    question_text = dialog_font.render(current_question["question"], True, (255, 255, 255))
    screen.blit(question_text, (120, HEIGHT - 320))
    
    # Draw options
    for i, option in enumerate(current_question["options"]):
        option_rect = pygame.Rect(120 + (i * 300), HEIGHT - 250, 250, 50)
        pygame.draw.rect(screen, (100, 100, 180), option_rect)
        pygame.draw.rect(screen, (255, 255, 255), option_rect, 2)
        
        option_text = dialog_font.render(str(option), True, (255, 255, 255))
        screen.blit(option_text, (option_rect.centerx - option_text.get_width()//2, 
                                  option_rect.centery - option_text.get_height()//2))

def draw_feedback():
    # Draw feedback box
    feedback_box = pygame.Surface((WIDTH - 400, 80), pygame.SRCALPHA)
    feedback_box.fill((0, 0, 0, 200))
    screen.blit(feedback_box, (200, 200))
    
    # Draw feedback text
    feedback_text = font.render(answer_feedback, True, (255, 255, 255))
    screen.blit(feedback_text, (WIDTH // 2 - feedback_text.get_width() // 2, 240))

# Game loop
running = True
while running:
    dt = clock.tick(60)
    screen.blit(background, (0, 0))

    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            running = False
        elif event.type == pygame.KEYDOWN:
            if event.key == pygame.K_ESCAPE:
                if selection_mode:
                    running = False
                else:
                    return_to_selection()
            
            if event.key == pygame.K_SPACE:
                if dialog_active:
                    next_dialog()
                elif not selection_mode and on_ground and not showing_feedback and not switching_stage:
                    jump_velocity = jump_strength
                    on_ground = False
            
            # Character selection controls
            if selection_mode:
                if event.key == pygame.K_LEFT or event.key == pygame.K_a:
                    current_character_index = (current_character_index - 1) % len(characters)
                elif event.key == pygame.K_RIGHT or event.key == pygame.K_d:
                    current_character_index = (current_character_index + 1) % len(characters)
                elif event.key == pygame.K_RETURN or event.key == pygame.K_SPACE:
                    select_character(current_character_index)
                    selection_mode = False
                    show_dialog(intro_dialog)
        
        elif event.type == pygame.MOUSEBUTTONDOWN:
            if selection_mode:
                # Check if arrows were clicked
                if left_arrow.collidepoint(event.pos):
                    current_character_index = (current_character_index - 1) % len(characters)
                elif right_arrow.collidepoint(event.pos):
                    current_character_index = (current_character_index + 1) % len(characters)
                
                # Check if character was clicked for selection
                char_rect = characters[current_character_index]["img"].get_rect(
                    center=(WIDTH // 2, HEIGHT // 2 - 50)
                )
                if char_rect.collidepoint(event.pos):
                    select_character(current_character_index)
                    selection_mode = False
                    show_dialog(intro_dialog)
            
            # Check for question answer selection (only when not showing feedback)
            elif current_question and not question_answered and not showing_feedback and not switching_stage:
                for i, option in enumerate(current_question["options"]):
                    option_rect = pygame.Rect(120 + (i * 300), HEIGHT - 250, 250, 50)
                    if option_rect.collidepoint(event.pos):
                        check_answer(option)

    if selection_mode:
        # Draw semi-transparent overlay
        overlay = pygame.Surface((WIDTH, HEIGHT), pygame.SRCALPHA)
        overlay.fill((0, 0, 0, 160))  # Darker overlay for better contrast with background
        screen.blit(overlay, (0, 0))
        
        # Draw heading with simpler style
        title = font.render("CHOOSE YOUR HERO", True, (255, 255, 255))
        title_shadow = font.render("CHOOSE YOUR HERO", True, (0, 0, 0))
        screen.blit(title_shadow, (WIDTH // 2 - title.get_width() // 2 + 2, 42))
        screen.blit(title, (WIDTH // 2 - title.get_width() // 2, 40))
        
        # Draw character in spotlight
        current_char = characters[current_character_index]["img"]
        char_rect = current_char.get_rect(center=(WIDTH // 2, HEIGHT // 2 - 50))
        
        # Draw spotlight effect
        spotlight = pygame.Surface((WIDTH, HEIGHT), pygame.SRCALPHA)
        pygame.draw.circle(spotlight, (255, 255, 255, 100), char_rect.center, 150)
        screen.blit(spotlight, (0, 0))
        
        # Draw character
        screen.blit(current_char, char_rect)
        
        # Draw character name
        name_text = small_font.render(characters[current_character_index]["name"], True, (255, 255, 255))
        screen.blit(name_text, (WIDTH // 2 - name_text.get_width() // 2, HEIGHT // 2 + 100))
        
        # Draw navigation arrows
        pygame.draw.polygon(screen, (255, 255, 255), [
            (left_arrow.left + 10, left_arrow.centery),
            (left_arrow.right - 10, left_arrow.top + 10),
            (left_arrow.right - 10, left_arrow.bottom - 10)
        ])
        
        pygame.draw.polygon(screen, (255, 255, 255), [
            (right_arrow.right - 10, right_arrow.centery),
            (right_arrow.left + 10, right_arrow.top + 10),
            (right_arrow.left + 10, right_arrow.bottom - 10)
        ])
        
        # Draw selection instructions
        instructions = small_font.render("Use LEFT/RIGHT arrows to browse, ENTER to select", True, (255, 255, 255))
        screen.blit(instructions, (WIDTH // 2 - instructions.get_width() // 2, HEIGHT - 100))
        
        # Show character count
        count_text = small_font.render(f"{current_character_index + 1}/{len(characters)}", True, (255, 255, 255))
        screen.blit(count_text, (WIDTH // 2 - count_text.get_width() // 2, HEIGHT - 60))
        
    else:
        # Game play mode
        keys = pygame.key.get_pressed()
        moving = False

        # Process feedback timer
        if showing_feedback:
            feedback_timer -= 1
            if feedback_timer <= 0:
                showing_feedback = False
                
                # Trigger next sequence after feedback expires
                if stage_complete:
                    if game_stage == 1:
                        show_dialog(npc_success)
                    elif game_stage == 2:
                        show_dialog(cavern_npc_success)
                elif question_answered:
                    if game_stage == 1:
                        show_dialog(npc_next_question)
                    elif game_stage == 2:
                        show_dialog(cavern_npc_next_question)
                    current_question = None  # Clear the current question

        # Always allow movement except in very specific states
        can_move = not (dialog_active or showing_feedback or 
                      (current_question and not question_answered) or 
                      switching_stage)
                      
        if can_move:
            if keys[pygame.K_LEFT] or keys[pygame.K_a]:
                player_rect.x -= speed
                facing = "left"
                moving = True
            if keys[pygame.K_RIGHT] or keys[pygame.K_d]:
                player_rect.x += speed
                facing = "right"
                moving = True
            
            # Apply gravity and jumping
            if not on_ground:
                player_rect.y += jump_velocity
                jump_velocity += gravity
                
                # Check if player has landed
                if player_rect.bottom >= GROUND_LEVEL:
                    player_rect.bottom = GROUND_LEVEL
                    on_ground = True
                    jump_velocity = 0

        # Constrain player to screen bounds
        player_rect.x = max(0, min(WIDTH - player_rect.width, player_rect.x))
        
        # Check for NPC interaction (Stage 1)
        if game_stage == 1 and not dialog_active and not current_question and not stage_complete and not showing_feedback:
            if player_rect.colliderect(npc_rect) and intro_shown:
                if questions_completed < len(stage1_questions):
                    if not question_answered:
                        if questions_completed == 0:
                            show_dialog(npc_greeting)
                        else:
                            show_dialog(npc_next_question)
                else:
                    show_dialog(npc_success)
        
        # Check for NPC interaction (Stage 2)
        if game_stage == 2 and not dialog_active and not current_question and not stage_complete and not showing_feedback:
            if player_rect.colliderect(cavern_npc_rect) and stage2_intro_shown:
                if questions_completed < len(stage2_questions):
                    if not question_answered:
                        if questions_completed == 0:
                            show_dialog(cavern_npc_greeting)
                        else:
                            show_dialog(cavern_npc_next_question)
                else:
                    show_dialog(cavern_npc_success)
        
        # Simulated bounce effect for walking animation
        if on_ground:
            bounce_timer += 0.2 if moving else 0
            offset = int(2 * abs(math.sin(bounce_timer)))
        else:
            offset = 0

        # Draw NPC (only in stage 1)
        if game_stage == 1:
            screen.blit(npc_img, npc_rect)
        
        # Draw Cavern NPC (only in stage 2)
        if game_stage == 2:
            screen.blit(cavern_npc_img, cavern_npc_rect)
        
        # Draw stage indicator
        if game_stage == 1:
            stage_text = font.render("Stage 1: Whispering Woods of Harmony", True, (255, 255, 255))
        else:  # game_stage == 2
            stage_text = font.render("Stage 2: Mystic Caverns of Multiplication", True, (255, 255, 255))
            
        screen.blit(stage_text, (20, 20))
        
        # Draw progress indicator
        if game_stage == 1:
            progress_text = small_font.render(f"Challenges: {questions_completed}/{len(stage1_questions)}", True, (255, 255, 255))
        else:  # game_stage == 2
            progress_text = small_font.render(f"Challenges: {questions_completed}/{len(stage2_questions)}", True, (255, 255, 255))
            
        screen.blit(progress_text, (20, 60))
        
        # Apply player offset for bounce animation
        draw_pos = player_rect.copy()
        draw_pos.y -= offset
        
        # Draw player
        screen.blit(player_img, draw_pos)
        
        # Show intro dialog if needed
        if show_intro and not intro_shown and not dialog_active:
            show_dialog(intro_dialog)

        # Show stage 2 intro when first entering cavern and not already showing dialog
        if game_stage == 2 and not stage2_intro_shown and not dialog_active and not current_question:
            show_dialog(stage2_intro)
        
        # Draw active dialog box
        if dialog_active:
            draw_dialog_box()
        
        # Draw current question
        if current_question and not question_answered and not showing_feedback:
            draw_question_box()
        
        # Draw feedback if needed
        if showing_feedback:
            draw_feedback()

    # Update display
    pygame.display.flip()

# Quit pygame
pygame.quit()
sys.exit()
