# OOP-Assignment

# Assignment 1: Design Your Own Class! 🏗️


# Create a class representing anything you like (a Smartphone, Book, or even a Superhero!).

class Smartphone:
    def __init__(self, brand, model, storage_gb, battery_mah, os="Android"):
        """
        Initialize a smartphone with basic specifications
        :param brand: Manufacturer (e.g., Samsung, Apple)
        :param model: Device model (e.g., Galaxy S23, iPhone 15)
        :param storage_gb: Internal storage in GB
        :param battery_mah: Battery capacity in mAh
        :param os: Operating system (default: Android)
        """
        self.brand = brand
        self.model = model
        self.storage_gb = storage_gb
        self.battery_mah = battery_mah
        self.os = os
        self.is_powered_on = False
        self.installed_apps = []
        self.battery_level = 100  # Percentage
        self.storage_used = 0  # GB

    def power_on(self):
        """Turn on the smartphone"""
        if not self.is_powered_on:
            self.is_powered_on = True
            return f"{self.brand} {self.model} is now powered on"
        return "Device is already on"

    def power_off(self):
        """Turn off the smartphone"""
        if self.is_powered_on:
            self.is_powered_on = False
            return f"{self.brand} {self.model} is now powered off"
        return "Device is already off"

    def install_app(self, app_name, size_gb):
        """Install an application if enough storage is available"""
        if not self.is_powered_on:
            return "Cannot install apps while device is off"
        
        if self.storage_used + size_gb > self.storage_gb:
            return f"Not enough storage to install {app_name}"
        
        self.installed_apps.append(app_name)
        self.storage_used += size_gb
        return f"{app_name} installed successfully"

    def uninstall_app(self, app_name):
        """Remove an installed application"""
        if app_name in self.installed_apps:
            self.installed_apps.remove(app_name)
            # Note: In real implementation, we'd track each app's size
            return f"{app_name} uninstalled"
        return "App not found"

    def check_battery(self):
        """Check current battery level"""
        return f"Battery level: {self.battery_level}%"

    def charge(self, minutes):
        """Simulate charging the battery"""
        charge_per_minute = 0.5  # Percentage per minute
        new_level = self.battery_level + (charge_per_minute * minutes)
        self.battery_level = min(new_level, 100)
        return f"Charged for {minutes} minutes. Battery now at {self.battery_level}%"

    def use_battery(self, percentage):
        """Simulate battery usage"""
        self.battery_level = max(self.battery_level - percentage, 0)
        if self.battery_level == 0:
            self.is_powered_on = False
            return "Battery depleted! Device has shut down"
        return f"Used {percentage}% battery. Remaining: {self.battery_level}%"

    def get_specs(self):
        """Display all specifications"""
        return (f"Brand: {self.brand}\nModel: {self.model}\nOS: {self.os}\n"
                f"Storage: {self.storage_gb}GB ({self.storage_used}GB used)\n"
                f"Battery: {self.battery_mah}mAh\n"
                f"Installed Apps: {', '.join(self.installed_apps) if self.installed_apps else 'None'}")

# Example Usage
if __name__ == "__main__":
    my_phone = Smartphone("Samsung", "Galaxy S23", 256, 3900, "Android")
    
    print(my_phone.power_on())
    print(my_phone.install_app("WhatsApp", 0.5))
    print(my_phone.install_app("Instagram", 1.2))
    print(my_phone.use_battery(30))
    print(my_phone.charge(20))
    print("\nDevice Specifications:")
    print(my_phone.get_specs())
    print(my_phone.power_off())


# Add attributes and methods to bring the class to life!

import random
from datetime import datetime

class Smartphone:
    def __init__(self, brand, model, storage_gb, battery_mah, os="Android", screen_size=6.1):
        self.brand = brand
        self.model = model
        self.storage_gb = storage_gb
        self.battery_mah = battery_mah
        self.os = os
        self.screen_size = screen_size
        self.is_powered_on = False
        self.is_locked = True
        self.battery_level = 100
        self.storage_used = 0
        self.installed_apps = []
        self.contacts = {}
        self.current_call = None
        self.brightness = 50
        self.volume = 70
        self.wallpaper = "default"
        self.last_charged = None
        self.network = "disconnected"
        self.bluetooth = False
        self.wifi = False
        self.gps = False

    def power_on(self):
        if not self.is_powered_on:
            self.is_powered_on = True
            self.network = "connected" if random.random() > 0.2 else "no signal"
            return f"📱 {self.brand} {self.model} boots up"
        return "Device is already on"

    def power_off(self):
        if self.is_powered_on:
            self.is_powered_on = False
            self.is_locked = True
            self.current_call = None
            self.network = "disconnected"
            return "Device shuts down"
        return "Device is already off"

    def unlock(self, method="pin", credential="1234"):
        if not self.is_powered_on:
            return "Can't unlock - device is off"
        
        if method == "pin" and credential == "1234":
            self.is_locked = False
            return "🔓 Device unlocked"
        elif method == "fingerprint":
            if random.random() > 0.1:  # 90% success rate
                self.is_locked = False
                return "👆 Fingerprint recognized"
            return "Fingerprint not recognized"
        return "❌ Unlock failed"

    def lock(self):
        self.is_locked = True
        return "🔒 Device locked"

    def make_call(self, contact_name):
        if not self.is_powered_on:
            return "Device is off"
        if self.is_locked:
            return "Unlock device first"
        if self.battery_level < 5:
            return "Low battery - cannot make call"
        
        if contact_name in self.contacts:
            self.current_call = contact_name
            self.battery_level = max(self.battery_level - 1, 0)
            return f"📞 Calling {contact_name}..."
        return "Contact not found"

    def end_call(self):
        if self.current_call:
            call_duration = random.randint(10, 300)
            ended_call = self.current_call
            self.current_call = None
            return f"Call with {ended_call} ended after {call_duration} seconds"
        return "No active call"

    def install_app(self, app_name, size_gb):
        if not self.is_powered_on:
            return "Power on device first"
        if self.is_locked:
            return "Unlock device first"
        
        if self.storage_used + size_gb > self.storage_gb:
            return f"❌ Not enough space for {app_name}"
        
        self.installed_apps.append(app_name)
        self.storage_used += size_gb
        return f"⬇️ Installed {app_name} ({size_gb}GB)"

    def take_photo(self, mode="standard"):
        if not self.is_powered_on:
            return "Device is off"
        if self.is_locked:
            return "Unlock device first"
        
        photo_size = random.uniform(1.5, 5.0)  # MB
        self.storage_used += photo_size / 1024  # Convert to GB
        modes = {
            "standard": "📷 Standard photo taken",
            "portrait": "👤 Portrait mode photo taken",
            "night": "🌃 Night mode photo taken"
        }
        return modes.get(mode, "Photo taken")

    def check_notifications(self):
        notifications = []
        if random.random() > 0.7:
            notifications.append("📧 New email")
        if random.random() > 0.5:
            notifications.append("💬 New message")
        if random.random() > 0.9:
            notifications.append("⚠️ System update available")
        return notifications if notifications else "No new notifications"

    def charge(self, minutes):
        charge_rate = 0.8  # % per minute
        self.battery_level = min(self.battery_level + (minutes * charge_rate), 100)
        self.last_charged = datetime.now().strftime("%H:%M")
        return f"⚡ Charged for {minutes} minutes ({self.battery_level:.0f}%)"

    def set_wallpaper(self, name):
        self.wallpaper = name
        return f"🎨 Wallpaper set to {name}"

    def toggle_wifi(self):
        self.wifi = not self.wifi
        return f"WiFi {'✅ on' if self.wifi else '❌ off'}"

    def __str__(self):
        status = "on 🔵" if self.is_powered_on else "off ⚫"
        locked = "locked 🔒" if self.is_locked else "unlocked 🔓"
        return (
            f"{self.brand} {self.model}\n"
            f"Status: {status} | {locked}\n"
            f"OS: {self.os} | Battery: {self.battery_level:.0f}%\n"
            f"Storage: {self.storage_used:.1f}/{self.storage_gb}GB\n"
            f"Network: {self.network} | WiFi: {'on' if self.wifi else 'off'}\n"
            f"Current call: {self.current_call or 'None'}"
        )

# Example Usage
if __name__ == "__main__":
    phone = Smartphone("Apple", "iPhone 15 Pro", 256, 3274, "iOS", 6.1)
    phone.contacts = {"Mom": "555-1234", "Work": "555-5678"}
    
    print(phone.power_on())
    print(phone.unlock("fingerprint"))
    print(phone.make_call("Mom"))
    print(phone.take_photo("portrait"))
    print(phone.install_app("TikTok", 1.2))
    print(phone.toggle_wifi())
    print("\n", phone, "\n")
    print(phone.check_notifications())
    print(phone.end_call())
    print(phone.power_off())




# Use constructors to initialize each object with unique values.

import random
from datetime import datetime
from enum import Enum

class PhoneBrand(Enum):
    APPLE = "Apple"
    SAMSUNG = "Samsung"
    GOOGLE = "Google"
    ONEPLUS = "OnePlus"
    XIAOMI = "Xiaomi"

class Smartphone:
    # Class attributes with default values for randomization
    BRAND_MODELS = {
        PhoneBrand.APPLE: ["iPhone 15", "iPhone 15 Pro", "iPhone 14", "iPhone SE"],
        PhoneBrand.SAMSUNG: ["Galaxy S23", "Galaxy Z Flip", "Galaxy A54", "Galaxy Note"],
        PhoneBrand.GOOGLE: ["Pixel 8", "Pixel 7a", "Pixel Fold", "Pixel 6a"],
        PhoneBrand.ONEPLUS: ["11 Pro", "10T", "Nord 3", "Open"],
        PhoneBrand.XIAOMI: ["13 Pro", "12T", "Redmi Note 12", "Mix Fold 2"]
    }
    
    OS_VERSIONS = {
        PhoneBrand.APPLE: "iOS 17",
        PhoneBrand.SAMSUNG: "Android 13",
        PhoneBrand.GOOGLE: "Android 14",
        PhoneBrand.ONEPLUS: "Android 13",
        PhoneBrand.XIAOMI: "Android 13"
    }
    
    STORAGE_OPTIONS = [64, 128, 256, 512, 1024]
    BATTERY_RANGES = {
        "small": (3000, 3500),
        "medium": (3501, 4500),
        "large": (4501, 5500)
    }
    
    def __init__(self, brand, model, storage_gb, battery_mah, os, screen_size, imei):
        """Constructor with mandatory unique parameters"""
        self.brand = brand
        self.model = model
        self.storage_gb = storage_gb
        self.battery_mah = battery_mah
        self.os = os
        self.screen_size = screen_size
        self.imei = imei  # Unique identifier for each phone
        
        # Initialize dynamic properties
        self.is_powered_on = False
        self.is_locked = True
        self.battery_level = random.randint(20, 100)
        self.storage_used = 0
        self.installed_apps = ["Phone", "Messages", "Camera"]
        self.contacts = {}
        self.current_call = None
        self.brightness = 50
        self.volume = 70
        self.wallpaper = "default"
        self.last_charged = None
        self.network = "disconnected"
        self.bluetooth = False
        self.wifi = False
        self.gps = False
        self.creation_date = datetime.now()
        
        # Generate some random contacts
        self._generate_contacts()
    
    @classmethod
    def create_random_phone(cls):
        """Factory method to create a phone with randomized specs"""
        brand = random.choice(list(PhoneBrand))
        model = random.choice(cls.BRAND_MODELS[brand])
        storage = random.choice(cls.STORAGE_OPTIONS)
        
        # Determine battery size based on model type
        if "Pro" in model or "Ultra" in model:
            battery_size = random.randint(*cls.BATTERY_RANGES["large"])
        elif "Mini" in model or "SE" in model:
            battery_size = random.randint(*cls.BATTERY_RANGES["small"])
        else:
            battery_size = random.randint(*cls.BATTERY_RANGES["medium"])
        
        # Generate screen size based on model type
        if "Fold" in model or "Flip" in model:
            screen_size = round(random.uniform(6.5, 7.6), 1
        else:
            screen_size = round(random.uniform(5.8, 6.7), 1
        
        # Generate unique IMEI (simplified)
        imei = f"{random.randint(1000000, 9999999)}-{random.randint(1000000, 9999999)}"
        
        return cls(
            brand=brand.value,
            model=model,
            storage_gb=storage,
            battery_mah=battery_size,
            os=cls.OS_VERSIONS[brand],
            screen_size=screen_size,
            imei=imei
        )
    
    def _generate_contacts(self):
        """Generate random contacts for the phone"""
        first_names = ["John", "Emma", "Michael", "Sophia", "James", "Olivia"]
        last_names = ["Smith", "Johnson", "Williams", "Brown", "Jones", "Garcia"]
        
        for i in range(random.randint(5, 15)):
            name = f"{random.choice(first_names)} {random.choice(last_names)}"
            phone = f"{random.randint(200, 999)}-{random.randint(100, 999)}-{random.randint(1000, 9999)}"
            self.contacts[name] = phone
    
    def __str__(self):
        return (
            f"{self.brand} {self.model} (IMEI: {self.imei})\n"
            f"OS: {self.os} | Storage: {self.storage_gb}GB\n"
            f"Screen: {self.screen_size}\" | Battery: {self.battery_mah}mAh\n"
            f"Created: {self.creation_date.strftime('%Y-%m-%d %H:%M')}"
        )

# Example usage
if __name__ == "__main__":
    # Create specific phone
    phone1 = Smartphone(
        brand="Apple",
        model="iPhone 15 Pro",
        storage_gb=256,
        battery_mah=3274,
        os="iOS 17",
        screen_size=6.1,
        imei="1234567-7654321"
    )
    
    print("Custom phone:")
    print(phone1)
    
    # Create random phones
    print("\nRandom phones:")
    for _ in range(3):
        phone = Smartphone.create_random_phone()
        print(phone)
        print("---")




# Add an inheritance layer to explore polymorphism or encapsulation.

import random
from datetime import datetime
from enum import Enum
from abc import ABC, abstractmethod

class PhoneBrand(Enum):
    APPLE = "Apple"
    SAMSUNG = "Samsung"
    GOOGLE = "Google"
    ONEPLUS = "OnePlus"
    XIAOMI = "Xiaomi"

class MobileDevice(ABC):
    """Abstract base class for all mobile devices"""
    def __init__(self, brand, model, os, imei):
        self._brand = brand  # Encapsulated attribute
        self._model = model
        self._os = os
        self._imei = imei  # Should be immutable
        self._is_powered_on = False
        self._creation_date = datetime.now()
    
    @property
    def brand(self):
        return self._brand
    
    @property
    def model(self):
        return self._model
    
    @property
    def imei(self):
        return self._imei
    
    @property
    def full_name(self):
        return f"{self._brand} {self._model}"
    
    @abstractmethod
    def power_on(self):
        pass
    
    @abstractmethod
    def power_off(self):
        pass
    
    def get_age(self):
        return (datetime.now() - self._creation_date).days

class Smartphone(MobileDevice):
    """Concrete smartphone implementation with polymorphism"""
    def __init__(self, brand, model, os, imei, storage_gb, battery_mah, screen_size):
        super().__init__(brand, model, os, imei)
        self._storage_gb = storage_gb
        self._battery_mah = battery_mah
        self._screen_size = screen_size
        self._is_locked = True
        self._battery_level = random.randint(20, 100)
        self._storage_used = 0
        self._installed_apps = ["Phone", "Messages", "Camera"]
        self._contacts = {}
        self._generate_contacts()
    
    # Encapsulated properties with validation
    @property
    def battery_level(self):
        return self._battery_level
    
    @battery_level.setter
    def battery_level(self, value):
        if 0 <= value <= 100:
            self._battery_level = value
        else:
            raise ValueError("Battery level must be between 0-100%")
    
    def power_on(self):
        if not self._is_powered_on:
            self._is_powered_on = True
            return f"📱 {self.full_name} boots up"
        return "Device is already on"
    
    def power_off(self):
        if self._is_powered_on:
            self._is_powered_on = False
            self._is_locked = True
            return "Device shuts down"
        return "Device is already off"
    
    def unlock(self, method="pin"):
        if not self._is_powered_on:
            return "Can't unlock - device is off"
        if method == "pin":
            self._is_locked = False
            return "🔓 Device unlocked"
        return "❌ Unlock failed"
    
    def install_app(self, app_name, size_gb):
        if self._storage_used + size_gb > self._storage_gb:
            return f"❌ Not enough space for {app_name}"
        self._installed_apps.append(app_name)
        self._storage_used += size_gb
        return f"⬇️ Installed {app_name}"
    
    def __str__(self):
        return (f"{self.full_name}\n"
                f"OS: {self._os} | Storage: {self._storage_gb}GB\n"
                f"Battery: {self._battery_mah}mAh ({self._battery_level}%)\n"
                f"Age: {self.get_age()} days")

class FoldableSmartphone(Smartphone):
    """Specialized smartphone with polymorphic behavior"""
    def __init__(self, brand, model, os, imei, storage_gb, battery_mah, screen_size, folded_size):
        super().__init__(brand, model, os, imei, storage_gb, battery_mah, screen_size)
        self._folded_size = folded_size
        self._is_folded = False
    
    def toggle_fold(self):
        self._is_folded = not self._is_folded
        return f"📱 {'Folded' if self._is_folded else 'Unfolded'}"
    
    def power_on(self):  # Polymorphic override
        if not self._is_powered_on:
            self._is_powered_on = True
            return f"📱 {self.full_name} unfolds and boots up"
        return "Device is already on"
    
    def __str__(self):
        return (f"{super().__str__()}\n"
                f"Screen: {self._screen_size}\" ({self._folded_size}\" when folded)\n"
                f"Status: {'Folded' if self._is_folded else 'Unfolded'}")

class Tablet(MobileDevice):
    """Another mobile device demonstrating polymorphism"""
    def __init__(self, brand, model, os, imei, screen_size, has_cellular):
        super().__init__(brand, model, os, imei)
        self._screen_size = screen_size
        self._has_cellular = has_cellular
        self._current_app = None
    
    def power_on(self):
        if not self._is_powered_on:
            self._is_powered_on = True
            return f"⌚ {self.full_name} tablet powers on"
        return "Tablet is already on"
    
    def power_off(self):
        if self._is_powered_on:
            self._is_powered_on = False
            return "Tablet shuts down"
        return "Tablet is already off"
    
    def launch_app(self, app_name):
        self._current_app = app_name
        return f"🚀 Launching {app_name} in tablet mode"
    
    def __str__(self):
        return (f"{self.full_name} Tablet\n"
                f"Screen: {self._screen_size}\"\n"
                f"Cellular: {'Yes' if self._has_cellular else 'WiFi-only'}\n"
                f"Age: {self.get_age()} days")

def demonstrate_polymorphism(devices):
    """Show polymorphic behavior across device types"""
    print("\n=== Device Demonstration ===")
    for device in devices:
        print("\n" + "="*40)
        print(f"Testing {device.full_name}:")
        print(device.power_on())
        
        if isinstance(device, Smartphone):
            print(device.unlock())
            print(device.install_app("Polymorphism Demo", 0.5))
        elif isinstance(device, Tablet):
            print(device.launch_app("Netflix"))
        
        if isinstance(device, FoldableSmartphone):
            print(device.toggle_fold())
        
        print(device)
        print(device.power_off())

# Create devices
devices = [
    Smartphone("Apple", "iPhone 15", "iOS 17", "123-456", 128, 3349, 6.1),
    FoldableSmartphone("Samsung", "Galaxy Z Fold", "Android 13", "789-012", 256, 4400, 7.6, 6.2),
    Tablet("Apple", "iPad Pro", "iPadOS 17", "345-678", 12.9, True)
]


# Demonstrate polymorphism
demonstrate_polymorphism(devices)

=== Device Demonstration ===

========================================
Testing Apple iPhone 15:
📱 Apple iPhone 15 boots up
🔓 Device unlocked
⬇️ Installed Polymorphism Demo
Apple iPhone 15
OS: iOS 17 | Storage: 128GB
Battery: 3349mAh (82%)
Age: 0 days
Device shuts down

========================================
Testing Samsung Galaxy Z Fold:
📱 Samsung Galaxy Z Fold unfolds and boots up
🔓 Device unlocked
⬇️ Installed Polymorphism Demo
📱 Unfolded
Samsung Galaxy Z Fold
OS: Android 13 | Storage: 256GB
Battery: 4400mAh (37%)
Age: 0 days
Screen: 7.6" (6.2" when folded)
Status: Unfolded
Device shuts down

========================================
Testing Apple iPad Pro:
⌚ Apple iPad Pro tablet powers on
🚀 Launching Netflix in tablet mode
Apple iPad Pro Tablet
Screen: 12.9"
Cellular: Yes
Age: 0 days
Tablet shuts down



                     #  Activity 2: Polymorphism Challenge! 🎭
# Create a program that includes animals or vehicles with the same action (like move()). However, make each class define move() differently (for example, Car.move() prints "Driving" 🚗, while Plane.move() prints "Flying" ✈️)

  from abc import ABC, abstractmethod

class Movable(ABC):
    """Abstract base class for all movable objects"""
    @abstractmethod
    def move(self):
        pass

# ===== VEHICLES =====
class Car(Movable):
    def move(self):
        return "🚗 Driving on the road"

class Plane(Movable):
    def move(self):
        return "✈️ Flying through the sky"

class Boat(Movable):
    def move(self):
        return "⛵ Sailing on water"

class Bicycle(Movable):
    def move(self):
        return "🚴 Pedaling on bike paths"

# ===== ANIMALS =====
class Dog(Movable):
    def move(self):
        return "🐕 Running on four legs"

class Fish(Movable):
    def move(self):
        return "🐟 Swimming in water"

class Snake(Movable):
    def move(self):
        return "🐍 Slithering on the ground"

class Bird(Movable):
    def move(self):
        return "🦅 Soaring through the air"

# ===== DEMONSTRATION =====
def movement_demo(movable_objects):
    print("=== MOVEMENT DEMONSTRATION ===")
    for obj in movable_objects:
        print(f"{type(obj).__name__}: {obj.move()}")
    print()

# Create mixed list of vehicles and animals
objects = [
    Car(),
    Plane(),
    Dog(),
    Boat(),
    Fish(),
    Bicycle(),
    Snake(),
    Bird()
]

# Demonstrate polymorphic behavior
movement_demo(objects)

# Additional demonstration with user input
def interactive_demo():
    print("\n=== INTERACTIVE DEMO ===")
    print("Choose an object to move:")
    print("1. Car\n2. Plane\n3. Dog\n4. Boat\n5. Fish\n6. Bicycle\n7. Snake\n8. Bird")
    
    choices = {
        1: Car(),
        2: Plane(),
        3: Dog(),
        4: Boat(),
        5: Fish(),
        6: Bicycle(),
        7: Snake(),
        8: Bird()
    }
    
    while True:
        try:
            choice = int(input("Enter your choice (1-8) or 0 to exit: "))
            if choice == 0:
                break
            print(choices[choice].move())
        except (ValueError, KeyError):
            print("Invalid input! Please enter a number 1-8")

interactive_demo()

Program Output
=== MOVEMENT DEMONSTRATION ===
Car: 🚗 Driving on the road
Plane: ✈️ Flying through the sky
Dog: 🐕 Running on four legs
Boat: ⛵ Sailing on water
Fish: 🐟 Swimming in water
Bicycle: 🚴 Pedaling on bike paths
Snake: 🐍 Slithering on the ground
Bird: 🦅 Soaring through the air

=== INTERACTIVE DEMO ===
Choose an object to move:
1. Car
2. Plane
3. Dog
4. Boat
5. Fish
6. Bicycle
7. Snake
8. Bird
Enter your choice (1-8) or 0 to exit: 2
✈️ Flying through the sky
Enter your choice (1-8) or 0 to exit: 5
🐟 Swimming in water
Enter your choice (1-8) or 0 to exit: 0
