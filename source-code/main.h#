require "time"

def easter(year : Int32) : Tuple(Int32, Int32)
  a = year % 19
  b = year // 100
  c = year % 100
  d = b // 4
  e = b % 4
  f = (b + 8) // 25
  g = (b - f + 1) // 3
  h = (19 * a + b - d - g + 15) % 30
  i = c // 4
  k = c % 4
  l = (32 + 2 * e + 2 * i - h - k) % 7
  m = (a + 11 * h + 22 * l) // 451
  month = (h + l - 7 * m + 114) // 31
  day = ((h + l - 7 * m + 114) % 31) + 1
  {month, day}
end

struct Holiday
  property month : Int32
  property day : Int32
  property name : String

  def initialize(@month, @day, @name)
  end
end

def main
  now = Time.local
  year = now.year
  month = now.month
  day = now.day

  holidays = [
    Holiday.new(1, 1, "Nowy Rok"),
    Holiday.new(1, 6, "Święto Trzech Króli"),
    Holiday.new(2, 14, "Walentynki"),
    Holiday.new(3, 8, "Dzień Kobiet"),
    Holiday.new(5, 1, "Święto Pracy"),
    Holiday.new(5, 3, "Święto Konstytucji 3 Maja"),
    Holiday.new(5, 26, "Dzień Matki"),
    Holiday.new(6, 1, "Dzień Dziecka"),
    Holiday.new(8, 15, "Wniebowzięcie Najświętszej Maryi Panny / Święto Wojska Polskiego"),
    Holiday.new(11, 1, "Wszystkich Świętych"),
    Holiday.new(11, 11, "Narodowe Święto Niepodległości"),
    Holiday.new(11, 30, "Andrzejki"),
    Holiday.new(12, 6, "Mikołajki"),
    Holiday.new(12, 24, "Wigilia Bożego Narodzenia"),
    Holiday.new(12, 25, "Boże Narodzenie (pierwszy dzień)"),
    Holiday.new(12, 26, "Boże Narodzenie (drugi dzień)"),
    Holiday.new(12, 31, "Sylwester"),
  ]

  # Calculate movable holidays based on Easter
  easter_month, easter_day = easter(year)
  easter_date = Time.local(year, easter_month, easter_day)

  # Tłusty Czwartek: 52 days before Easter
  tlusty_date = easter_date - Time::Span.new(days: 52)
  holidays << Holiday.new(tlusty_date.month, tlusty_date.day, "Tłusty Czwartek")

  # Wielkanoc Sunday
  holidays << Holiday.new(easter_date.month, easter_date.day, "Wielkanoc (Niedziela Wielkanocna)")

  # Lany Poniedziałek (Easter Monday)
  lany_date = easter_date + Time::Span.new(days: 1)
  holidays << Holiday.new(lany_date.month, lany_date.day, "Wielkanoc (Lany Poniedziałek)")

  # Zesłanie Ducha Świętego (Zielone Świątki): 49 days after Easter
  zielone_date = easter_date + Time::Span.new(days: 49)
  holidays << Holiday.new(zielone_date.month, zielone_date.day, "Zesłanie Ducha Świętego (Zielone Świątki)")

  # Boże Ciało: 60 days after Easter
  boze_cialo_date = easter_date + Time::Span.new(days: 60)
  holidays << Holiday.new(boze_cialo_date.month, boze_cialo_date.day, "Boże Ciało")

  # Check for today's holidays
  todays_holidays = holidays.select { |h| h.month == month && h.day == day }

  unless todays_holidays.empty?
    names = todays_holidays.map(&.name).join(", ")
    message = "Dzisiaj jest #{names}!"
    Process.run("notify-send", ["HackerOS Events", message])
  end
end

main
