# django-df-calendar
A set of Django models and utilities to manage time periods, business hours and appointment slots

# Entities
TimePeriod
TimeSlot (configurable) = free/busy for picking appointment possibly multiple providers

travel_time expert = preparing_time food?

pre_time = preparing time, travel time
post_time = delivery time, travel time

# Bookings

(base: list(TimePeriod), updates: list(TimePeriod), replace_dimensions_fields, update_dimension_fields, comparator)
-> created, deleted, updated

dimension_fields:
    HotelBooking 
        replace - hotel_id, 
        update - booking_extras, checkin_time, checkout_time, employee_id
    TaxiBooking
        replace - taxi_id, hotel_booking__hotel_id
        update - employee_id

# TimePeriod

# BaseHours
start_date
end_date
start_time
end_time
weekday

# OpeningHours
# HolidayHours

# TimeSlot

pd.load_slots
    pd.load_slots_google_calender
    pd.load_slots_db
    pd.load_slots_odoo
