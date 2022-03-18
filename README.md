# Web_Scraper_for_Carousell
from bs4 import BeautifulSoup
import requests as req
import numpy as np
import pandas as pd

url = input(f'Please input the link here:')
response = req.get(url)
html_doc = response.text
soup = BeautifulSoup(html_doc,"html.parser")

postdates = soup.find_all(class_="D_hh M_et D_ff M_bZ D_hi M_eu D_hl M_ex D_hn M_ez D_hq M_eB D_hs M_eE D_AE M_oE D_hf")
postdate=[]
for i in postdates:
    postdate.append(i.string)

items = soup.find_all({"div", "p"},class_="D_hh M_et D_ff M_bZ D_hi M_eu D_hl M_ex D_hn M_ez D_hq M_eB D_ht M_eF D_he")
item=[]
for j in items:
    item.append(j.string)

prices = soup.find_all({"div","p"}, class_="D_hh M_et D_ff M_bZ D_hi M_eu D_hl M_ex D_hn M_ez D_hq M_eB D_hs M_eE D_hc")
price=[]
for k in prices:
    price.append(k.string)

users = soup.find_all({"div", "p"}, class_="D_hh M_et D_fh M_cb D_hi M_eu D_hl M_ex D_hn M_ez D_hq M_eB D_ht M_eF D_he")
user=[]
for l in users:
    user.append(l.string)


result = np.array([user,postdate, item, price])
result_transpose = result.transpose()
print(result_transpose)
DF = pd.DataFrame(result_transpose)
DF.to_csv("hotel_voucher.csv",encoding="utf_8_sig", header=["user","postdate","item","price"])
