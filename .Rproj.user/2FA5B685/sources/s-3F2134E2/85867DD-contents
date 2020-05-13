# Scraping:
# Leis municipais e estaduais a respeito do novo Coronavírus (COVID-19)

#  https://leismunicipais.com.br/coronavirus

# Author: Michel Szklo
# Date:  May-2020

##########################################################################




# =================================================================
# 0. Set-up
# =================================================================

rm(list=ls())

# packages
packages<-c('dplyr','httr','rvest')

to_install<-packages[!(packages %in% installed.packages()[,"Package"])]
if(length(to_install)>0) install.packages(to_install)

lapply(packages,require,character.only=TRUE)

# web site to be scraped
site <- httr::GET("https://leismunicipais.com.br/coronavirus")

# =================================================================
# 1. Scraping Municipios
# =================================================================

# extracting infos from html table
tabela_mun <- httr::content(site, encoding = "UTF-8") %>%
  rvest::html_nodes(xpath = '//*[@id="tabela_leis_municipais"]/tbody/tr/td') %>%
  rvest::html_text() %>%
  trimws()

df_mun <- as.data.frame(matrix (data= tabela_mun, ncol = 2, byrow = T))
df_mun <- df_mun %>% mutate(V2 = gsub("\n                                        ","",V2))

tabela_lei <- httr::content(site, encoding = "UTF-8") %>%
  rvest::html_nodes(xpath = '//*[@id="tabela_leis_municipais"]/tbody/tr/td/a') %>%
  rvest::html_text() %>%
  trimws()

df_lei <- as.data.frame(matrix (data= tabela_lei, ncol = 1, byrow = T))


tabela_link <- httr::content(site, encoding = "UTF-8") %>%
  rvest::html_nodes(xpath = '//*[@id="tabela_leis_municipais"]/tbody/tr/td/a') %>%
  rvest::html_attr('href') %>%
  trimws()


df_link <- as.data.frame(matrix (data= tabela_link, ncol = 1, byrow = T))


# consolidating dataframe
df <- bind_cols(df_mun,df_lei,df_link)
df <- df[c(1,3,2,4)]
colnames(df) <- c("mun","lei","desc","link")

df <- df %>% 
  mutate(mun = as.character(mun), lei = as.character(lei), desc = as.character(desc), link = as.character(link)) %>% 
  mutate(desc = substr(desc, nchar(lei) + 1, nchar(desc)), estado = substr(mun, nchar(mun) - 1, nchar(mun)), mun = substr(mun, 1, nchar(mun)-3))


# save as csv
write.csv2(df, "leis_mun.csv", row.names = F)

# =================================================================
# 2. Scraping Estados
# =================================================================

# extracting infos from html table
tabela_estado <- httr::content(site, encoding = "UTF-8") %>%
  rvest::html_nodes(xpath = '//*[@id="tabela_leis_estaduais"]/tbody/tr/td') %>%
  rvest::html_text() %>%
  trimws()

df_estado <- as.data.frame(matrix (data= tabela_estado, ncol = 2, byrow = T))
df_estado <- df_estado %>% mutate(V2 = gsub("\n                                        ","",V2))

tabela_lei <- httr::content(site, encoding = "UTF-8") %>%
  rvest::html_nodes(xpath = '//*[@id="tabela_leis_estaduais"]/tbody/tr/td/a') %>%
  rvest::html_text() %>%
  trimws()

df_lei <- as.data.frame(matrix (data= tabela_lei, ncol = 1, byrow = T))


tabela_link <- httr::content(site, encoding = "UTF-8") %>%
  rvest::html_nodes(xpath = '//*[@id="tabela_leis_estaduais"]/tbody/tr/td/a') %>%
  rvest::html_attr('href') %>%
  trimws()

df_link <- as.data.frame(matrix (data= tabela_link, ncol = 1, byrow = T))


# consolidating dataframe
df <- bind_cols(df_estado,df_lei,df_link)
df <- df[c(1,3,2,4)]
colnames(df) <- c("estado","lei","desc","link")


df <- df %>% 
  mutate(estado = as.character(estado), lei = as.character(lei), desc = as.character(desc), link = as.character(link)) %>% 
  mutate(desc = substr(desc, nchar(lei) + 1, nchar(desc)))


# save as csv
write.csv2(df, "leis_estado.csv",row.names = F)