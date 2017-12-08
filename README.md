import bs4
import requests
import csv


def save_to_csv(data, path='C:/Users/KlapTop/PycharmProjects/out/scraped_events.csv'):
    with open(path, 'w', encoding='utf-8') as output_file:
        output_writer = csv.writer(output_file)
        output_writer.writerow(['What', 'Where', 'When', 'How much'])

        for row in data:
            output_writer.writerow(row)

list = []
pages = []
whatElems = []
whereElems = []
whenElems = []
priceElems = []
allList = []

r = requests.get('http://46.101.108.154')
r.raise_for_status()
soup = bs4.BeautifulSoup(r.text, 'html5lib')

pages = soup.select('pre a')
print(len(pages))

def whatElement(a):
    return a[0::3]

def whereElement(a):
    return a[1::2]

def whenElement(a):
    return a[0::2]

for x in range(2, 82):
    list.append('http://46.101.108.154/' + pages[x].getText())

whatElemsList = []
for el in list:
    r = requests.get(el)
    r.raise_for_status()
    soup = bs4.BeautifulSoup(r.text, 'html5lib')
    elemsWhat = soup.select('h3 b')
    elemsWhere = soup.select('tr td span[class="notranslate"]')
    print(str(whatElement(elemsWhat)).split("<")[1::2])

    allList.append(str(whatElement(elemsWhat)).split("<")[1::2])
    allList.append(str(whereElement(elemsWhere)).split("<")[3][5:])
    allList.append((str(whenElement(elemsWhere)).split("<")[2][2:] + str(whenElement(elemsWhere)).split("<")[3][3:]))
    allList.append(str(whenElement(elemsWhere)).split("fee:")[1].split(")")[0])


save_to_csv(allList)


