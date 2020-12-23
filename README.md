№1
import json
import csv

lst = []
with open("JSON/cinema.json", 'r') as json_file:
        lst.append(json.load(json_file))

lst2 = [{} for i in lst[0]]

with open('JSON/cinema2.csv', 'w', newline='') as csvfile:
    fieldnames = ['ID', 'CommonName', "Address", 'WebSite', 'WorkingHours', "latitude", 'longitude']
    writer = csv.DictWriter(csvfile, fieldnames=fieldnames, delimiter=';')
    writer.writeheader()
    
    for i, dic in enumerate(lst[0]):
        lst2[i]["ID"] = dic['global_id']
        lst2[i]["CommonName"] = dic['CommonName']
        lst2[i]["Address"] = dic['ObjectAddress'][0]['Address']
        lst2[i]['WebSite'] = dic['WebSite']
        lst2[i]['WorkingHours'] = {i['DayWeek']:i['WorkHours'] for i in dic['WorkingHours']} 
        lst2[i]["latitude"] = dic['geoData']['coordinates'][0][0]
        lst2[i]['longitude'] = dic['geoData']['coordinates'][0][1]

        writer.writerow({'ID': dic['global_id'],
                        'CommonName': dic['CommonName'],
                        "Address" : dic['ObjectAddress'][0]['Address'],
                        'WebSite': dic['WebSite'],
                        'WorkingHours': {i['DayWeek']:i['WorkHours'] for i in dic['WorkingHours']}, 
                        "latitude": dic['geoData']['coordinates'][0][0],
                        'longitude': dic['geoData']['coordinates'][0][1]})
        

    
with open('JSON/inst1.json', 'w') as f:
    json.dump(lst2, f)
