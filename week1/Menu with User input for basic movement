import RPi.GPIO as GPIO
import time

# --- SETUP ---
GPIO.setmode(GPIO.BOARD)
GPIO.setwarnings(False)

# --- PIN CONFIGURATION ---
# Left Motor
IN1 = 13 
IN2 = 15 
ENA = 11  

# Right Motor
IN3 = 16 
IN4 = 18 
ENB = 12  

# Set pins as Outputs
GPIO.setup(IN1, GPIO.OUT)
GPIO.setup(IN2, GPIO.OUT)
GPIO.setup(ENA, GPIO.OUT)

GPIO.setup(IN3, GPIO.OUT)
GPIO.setup(IN4, GPIO.OUT)
GPIO.setup(ENB, GPIO.OUT)

# --- PWM SETUP ---
# Lower frequency (100Hz) provides more torque at low speeds for L298N
pwm_left = GPIO.PWM(ENA, 1000)
pwm_right = GPIO.PWM(ENB, 1000)

# Start PWM at 0% duty cycle (Stopped)
pwm_left.start(0)
pwm_right.start(0)

# --- MOVEMENT FUNCTIONS ---

def set_speed(speed):
    """Sets speed for both motors (0–100)"""
    speed = max(0, min(100, speed))
    pwm_left.ChangeDutyCycle(speed)
    pwm_right.ChangeDutyCycle(speed)


def stop():
    """Stops all motors"""
    print("Stopping")
    GPIO.output(IN1, False)
    GPIO.output(IN2, False)
    GPIO.output(IN3, False)
    GPIO.output(IN4, False)
    set_speed(0)


def move_forward(speed):
    """Move forward"""
    print(f"Forward | Speed: {speed}%")
    set_speed(speed)

    # Left motor forward
    GPIO.output(IN1, True)
    GPIO.output(IN2, False)

    # Right motor forward
    GPIO.output(IN3, False)
    GPIO.output(IN4, True)


def move_backward(speed):
    """Move backward"""
    print(f"Backward | Speed: {speed}%")
    set_speed(speed)

    # Left motor backward
    GPIO.output(IN1, False)
    GPIO.output(IN2, True)

    # Right motor backward
    GPIO.output(IN3, True)
    GPIO.output(IN4, False)


def turn_left(speed):
    """Turn left (spin in place)"""
    print(f"Left Turn | Speed: {speed}%")
    set_speed(speed)

    # Left motor backward
    GPIO.output(IN1, False)
    GPIO.output(IN2, True)

    # Right motor forward
    GPIO.output(IN3, False)
    GPIO.output(IN4, True)


def turn_right(speed):
    """Turn right (spin in place)"""
    print(f"Right Turn | Speed: {speed}%")
    set_speed(speed)

    # Left motor forward
    GPIO.output(IN1, True)
    GPIO.output(IN2, False)

    # Right motor backward
    GPIO.output(IN3, True)
    GPIO.output(IN4, False)
    
# --- MAIN TEST LOOP ---
if __name__ == "__main__":
    try:
        print("\n--- MANUAL ROBOT CONTROL MODE ---")
        print("f = forward | b = backward | l = left | r = right")
        print("s = stop | q = quit")
        print("Use this to calibrate 90° turns\n")

        while True:
            cmd = input("Enter command (f/b/l/r/s/q): ").lower()

            if cmd == "q":
                break

            if cmd == "s":
                stop()
                continue

            speed = float(input("Enter PWM speed (0–100): "))
            duration = float(input("Enter time (seconds): "))

            if cmd == "f":
                print("Moving Forward")
                move_forward(speed)
                time.sleep(duration)
                stop()

            elif cmd == "b":
                print("Moving Backward")
                move_backward(speed)
                time.sleep(duration)
                stop()

            elif cmd == "l":
                print("Turning Left")
                turn_left(speed)
                time.sleep(duration)
                stop()

            elif cmd == "r":
                print("Turning Right")
                turn_right(speed)
                time.sleep(duration)
                stop()

            else:
                print("Invalid command!")

            print("\n--- Movement Complete ---\n")

    except KeyboardInterrupt:
        print("\nEmergency Stop")
    finally:
        stop()
        pwm_left.stop()
        pwm_right.stop()
        GPIO.cleanup()
    
