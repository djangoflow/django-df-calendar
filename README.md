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

TimeSlot

- time_from: TimeField
- time_to: TimeField


WeekdayTimeSlot

- weekday
- time_from
- time_to


DateTimeSlot

- date
- time_from
- time_to


## Real

### OpenSchedule


OpenSchedule_WeekdayTimeSlot(WeekdayTimeSlot)

- open_schedule fk

OpenSchedule_DateTimeSlot(DateTimeSlot)

- open_schedule fk
- is_working: bool


OpenSchedule

- working_weekday_time_slots: many OpenScheduleWeekdayTimeSlot
- schedule_date_exceptions: many OpenScheduleDateTimeSlot  # for day-offs in business days and vice versa



### AvailableSlots


SlotSchedule_DateTimeSlot(DateTimeSlot):

- date
- time_from
- time_to
- status:
  - free
  - booked

SlotSchedule:

- open_schedule: OpenSchedule
- slot_duration: SlotSettings
- slot_delay: SlotSettings
- planning_limit: days. (case: you cannot see/book slots for more than 30 days in the future)




## methods

- check user time slots for the future
- check my slots for the future/past
- change slot_duration/slot_delay (case of already booked slots)
- change planning_limit (case of already booked slots)
- cancel booked slot (my slot/my booking)


## Other

- Library should only know if slot is free/booked. It will not contain context of who booked the slot. 
- We should add 'official holidays' preset for adding day-offs. 

