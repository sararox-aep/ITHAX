# please click on edit to to see all the code or copy and paste into processing, on preview the text is formatting incorrectly and i could not figure out how to chnage it.

# ITHAX

# ITHAX - small cube that goes around collecting points which ultimatly do nothing

score = 0

# where the point is
px = 300
py = 250

# where ithax is
x = 100.0
y = 100.0

def setup():
    size(900, 500)
    textSize(20)

def draw():
    global score, px, py, x, y # changing the variables from the top

    # t = how many seconds have passed
    t = millis() / 1000.0

    # "end" after 90 seconds
    if t > 90:
        background(0)
        fill(255) # black
        text("TOTAL POINTS: " + str(score), 300, 240)
        text("REDEEMED: 0", 300, 280)
        return #reutrn means stop

    # background gets darker over time
    shade = map(t, 0, 90, 120, 30) # grey to black
    background(shade)

    # move ithax toward the point (he gets slower as he gets more tired)
    ease = map(t, 0, 90, 0.08, 0.02) # smaller, slower jumps over time
    x = x + (px - x) * ease 
    y = y + (py - y) * ease # velocity zooms and then slows as he comes closer to the point

    # collect the point if ithax is close enough
    if dist(x, y, px, py) < 25: # dist is the space between two places
        value = 1 + int(t / 15)      # points are worth more over time
        score = score + value
        px = random(50, 650)         # new point appears somewhere else x must stop at 650 where the scoreboard begins 700
        py = random(50, 450)

    # draw the point
    noStroke()
    fill(255, 210, 0) # yellow
    ellipse(px, py, 15, 15)

    # draw ithax (he starts as blue, then fades to grey by the end)
    bodyColor = lerpColor(color(80, 200, 255), color(110, 110, 120), t / 90.0)
    fill(bodyColor)
    rect(x - 20, y - 20, 40, 40)

    # eyes
    fill(255)
    rect(x - 12, y - 10, 8, 8)
    rect(x + 4, y - 10, 8, 8)

    # mouth: happy smile, then flat straihjt mouth, then frown
    noFill()
    stroke(0)
    strokeWeight(2)
    if t < 30: # runs for 30 secs
        arc(x, y + 5, 16, 10, 0, PI) # going from 0 to PI draws the bottom half looks like a smile
    elif t < 60: 
        line(x - 8, y + 10, x + 8, y + 10) # from 30-60 secs the mouth is a flat line
    else:
        arc(x, y + 12, 16, 10, PI, TWO_PI) # looks like frown last 30 secs
    noStroke()

    # score displayedon the right side
    fill(20)
    rect(700, 0, 200, 500) # side panel
    fill(255, 210, 0)
    text("SCOREBOARD", 720, 40)
    fill(255)
    text("POINTS", 720, 100)
    textSize(40)
    text(str(score), 720, 150)
    textSize(20)
    text("REDEEMABLE:", 720, 250) # never changes
    text("0", 720, 280)

    # message changes over time and gets pushier
    fill(255, 210, 0)
    if t < 30:
        text("KEEP GOING", 720, 450)
    elif t < 60:
        text("DO NOT STOP", 720, 450)
    else:
        text("KEEP COLLECTING", 720, 450)
