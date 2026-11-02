"""
Crop Yield Monitoring System (Digital Public Goods compliant)
Domain: Agriculture
Author: Limkokwing University Student
Module: PROG211 - Object-Oriented Programming 1
"""

class Farmer:
    """Class representing an agricultural producer using anonymous IDs for privacy."""
    
    def __init__(self, farmer_id: str, region: str):
        self.farmer_id = farmer_id
        self.region = region

    def display_info(self) -> None:
        """Instance method to display farmer details."""
        print(f"Farmer ID: {self.farmer_id} | Region: {self.region}")


class CropRecord:
    """Class representing a yield record for a specific crop cycle."""
    
    # Class attribute to track total yield across all records
    total_national_yield_kg = 0.0

    def __init__(self, record_id: str, farmer: Farmer, crop_type: str, 
                 yield_kg: float, location_coords: tuple):
        self.record_id = record_id
        self.farmer = farmer
        self.crop_type = crop_type
        self.yield_kg = yield_kg
        # Task 2 Bonus: Tuple used for immutable geographical coordinates (Latitude, Longitude)
        self.location_coords = location_coords
        
        # Update class attribute aggregate
        CropRecord.total_national_yield_kg += yield_kg

    def display_record((self) -> None:
        """Instance method to display detailed crop yield information."""
        print(f"--- Record ID: {self.record_id} ---")
        self.farmer.display_info()
        print(f"Crop: {self.crop_type}")
        print(f"Yield: {self.yield_kg:.2f} kg")
        print(f"GPS Coordinates (Lat, Long): {self.location_coords}")
        print(f"Estimated Standard Bags (50kg): {self.calculate_bags(self.yield_kg)}")
        print("-" * 30)

    @classmethod
    def get_total_national_yield(cls) -> float:
        """Class method (@classmethod) returning aggregate yield across all instances."""
        return cls.total_national_yield_kg

    @staticmethod
    def calculate_bags(yield_kg: float, bag_weight: float = 50.0) -> float:
        """Static method (@staticmethod) performing conversion calculation."""
        if bag_weight <= 0:
            return 0.0
        return round(yield_kg / bag_weight, 2)


class AgricultureManager:
    """Manager class to handle data storage and program interactions."""
    
    def __init__(self):
        # Task 2: Dictionary data structure mapping record_id -> CropRecord object
        self.records_db = {}

    def add_record(self, record: CropRecord) -> None:
        """Adds a CropRecord object into the dictionary storage."""
        self.records_db[record.record_id] = record
        print(f"[SUCCESS] Record '{record.record_id}' added successfully.")

    def display_all_records(self) -> None:
        """Iterates through the dictionary and calls display instance methods."""
        if not self.records_db:
            print("[INFO] No crop yield records available.")
            return

        print("\n================ ALL CROP YIELD RECORDS ================")
        for record_id, record_obj in self.records_db.items():
            record_obj.display_record()


# =====================================================================
# PROGRAM EXECUTION / EXAMPLE USAGE
# =====================================================================
if __name__ == "__main__":
    manager = AgricultureManager()

    # Creating Farmer objects (Task 1: Object Creation)
    farmer1 = Farmer(farmer_id="FMR-101", region="Bo District")
    farmer2 = Farmer(farmer_id="FMR-102", region="Bombali District")

    # Creating CropRecord objects with Tuples for GPS coordinates (Task 1 & Task 2)
    record1 = CropRecord(
        record_id="REC-001",
        farmer=farmer1,
        crop_type="Cassava",
        yield_kg=1250.5,
        location_coords=(7.9647, -11.7383)  # Tuple (Lat, Long)
    )

    record2 = CropRecord(
        record_id="REC-002",
        farmer=farmer2,
        crop_type="Rice",
        yield_kg=3400.0,
        location_coords=(8.8833, -12.0500)  # Tuple (Lat, Long)
    )

    # Adding records to dictionary structure (Task 2)
    manager.add_record(record1)
    manager.add_record(record2)

    # Displaying stored records (Task 2 & Instance Methods)
    manager.display_all_records()

    # Calling Class Method (Task 3)
    total_yield = CropRecord.get_total_national_yield()
    print(f"GLOBAL AGGREGATE YIELD: {total_yield:.2f} kg")

    # Calling Static Method directly (Task 3)
    sample_bags = CropRecord.calculate_bags(500.0)
    print(f"STATIC CALCULATION TEST: 500 kg = {sample_bags} bags (50kg each)")

