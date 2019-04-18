import csv
from geopy.geocoders import Nominatim


geolocator = Nominatim(user_agent="wanyi_festival")

num_rows, num_skipped = 0, 0
line_counter = 0
with open('festival.csv', mode='r') as csv_file:
    with open('festival_cleaned.csv', mode='a') as csv_file2:
        csv_reader = csv.DictReader(csv_file)
        next(csv_reader)
        fieldnames = ['name', 'type', 'state', 'city', 'lat', 'long', 'status',
                      'founded_month', 'founded_year', 'summarize', 'cancel_reason',
                      'cancel_time', 'link']
        csv_writer = csv.DictWriter(csv_file2, fieldnames=fieldnames)
        #csv_writer.writeheader()

        for row in csv_reader:
            line_counter += 1
            if line_counter < 408:
                continue
            row["Type"] = row["Type"].split()[0]
            row["Status"] = "Active" if row["Status"].lower().startswith("active") else "Inactive"
            city = row["City"] + ", " + row["State"] + ", USA"
            location = geolocator.geocode(city)
            if location is None:
                num_skipped += 1
                if num_skipped % 10 == 0:
                    print('number skipped: ', num_skipped)
                continue
            num_rows += 1
            if num_rows % 10 == 0:
                print('number processed: %d/%d' % (num_rows, line_counter))
            #print(row)
            csv_writer.writerow({'name': row['\ufeffName'], 'type': row['Type'],
                             'state': row['State'], 'city': row['City'],
                             'lat': location.latitude, 'long': location.longitude,
                             'status': row['Status'], 'founded_month': row['Month'],
                             'founded_year': row['Founded'], 'summarize': row['Summarize'],
                             'cancel_time': row['Stop_Time'], 'cancel_reason': row["Cancel_Reason"],
                             'link': row["Link"]})
