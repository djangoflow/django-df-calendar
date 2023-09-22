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



# Use cases

- Generate available time slots for service/meeting booking. Slots could be any length 10 min/30min/1h30min/etc. Slots can have delay between (50min slot + 10 min rest). user can also have holidays, day-offs, working days + working hours. 
- Generate working hours for business, including holidays, day-offs, working days + working hours. 


So, setup should be the same for both cases:

- I can pick my working days, for example:  mon/tue/thu/fri 09:00-12:00 + 15:00-18:00
- I can pick preset of goverment holidays.
- I can set-up day-offs, date_from - date_to.
- I can change working days for the specific period date-from - date-to. 


For slots I also need to have use cases:

- I need to check my slots based on working hours for the specific day
- Somebody book slot. It's unavailable after it. 
- Somebody cancel slot booking.





# Model Schema


## Abstract

CalendarEntry(TimePeriod)
- priority = 1000 = actual bookings (busy), 3000 = exceptions (avail/busy), 4500 = public holidays (busy), 5000 = normal (avail/busy), 9000 default
- time_from: TimeField default 00:00:00
- time_to: TimeField default 23:59:59
- date =  nullable
- weekday nullable
- day_of_month nullable
- is_available = BooleanField default True

## Real

### OperatingHours


OperatingHours

- entries: many CalendarEntry


### AvailableSlots


Slot(CalendarEntry)

...

SlottedMixin:

- open_schedule: many CalendarEntry
- slot_duration: int
- slot_delay: int
- planning_limit: days. (case: you cannot see/book slots for more than 30 days in the future)
- slots: many to many Slots


## methods

- check user time slots for the future
- check my slots for the future/past
- change slot_duration/slot_delay (case of already booked slots)
- change planning_limit (case of already booked slots)
- cancel booked slot (my slot/my booking)


## Other

- Library should only know if slot is free/booked. It will not contain context of who booked the slot. 
- We should add 'official holidays' preset for adding day-offs. 

