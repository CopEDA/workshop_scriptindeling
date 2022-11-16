# ALGEMENE INFO SCRIPT ----------------------------------------------------
# Doel: script om voorbeeld van script-structuur te laten zien
# Subdoel: automatisch verwerken van aangeboden data tot grafieken
# Gemaakt door Bastiaen Boekelo en Ciska Overbeek, zomer 2022
# Output is overzicht van grafieken per locatie en parameter
# Korte uitleg ----
# (eventueel korte uitleg van wat bijvoorbeeld in hulpbestanden terug te 
# vinden is)

rm(list = ls())

# PATHS -------------------------------------------------------------------


# LADEN LIBRARIES ---------------------------------------------------------
# Alleen packages installeren als nog niet geinstalleerd
packages_list <- c("ggplot2", "Rcpp", "pdftools", "gridExtra", "tidyverse", "lubridate")
new_pcks <- packages_list[!(packages_list %in% installed.packages()[,"Package"])]
if (length(new_pcks)) {
  install.packages(new_pcks, dependencies = TRUE)
}
rm(packages_list, new_pcks)
# laad libraries
library(Rcpp)
library(ggplot2)
library(gridExtra)
library(pdftools)
library(tidyverse)
library(lubridate)


# LADEN FUNCTIES ----------------------------------------------------------
# source(...)


# INLEZEN DATA ------------------------------------------------------------
# hulpbestanden
cols <- read.csv("hulpbestanden/kolomnamen.csv", sep = ",")$kolom_naam
params <- read.csv("hulpbestanden/WNS_idents.csv", sep = ",")
locs <- read.csv("hulpbestanden/locaties.csv", sep = ",")
# months <- read.csv("hulpbestanden/maanden.csv", sep = ",")

# Definieer namen outputs, ook benodigd voor inlezen
start_txt           <- "KWA Monitoring"
extra_txt           <- paste0(" - Gemaakt op ", Sys.Date(), " door ", Sys.getenv("USERNAME"))
output_log          <- paste0("output/Grafieken/", start_txt, extra_txt, "_ERROR.txt")
output_pdf          <- paste0("output/Grafieken/", start_txt, extra_txt, ".pdf")
output_pdf_en_kaart <- paste0("output/Grafieken_met_kaart/", start_txt, " met kaart", extra_txt, ".pdf")

# inlezen csv's
csvs <- list.files("input", full.names = T, recursive = F, pattern = ".csv$")
for (i in 1:length(csvs)) {
  df_tmp <- read.csv(csvs[i])
  if ("Order.number" %in% names(df_tmp)) {
    if (i == 1) {
      df_raw <- df_tmp
    } else {
      df_raw <- rbind(df_raw, df_tmp)
    }
  } else {
    error_text <- paste0("\n\n\tOEPS! IN DIT BESTAND IS GEEN KOLOM 'Order.number' AANWEZIG:\n\n\t----->  ", csvs[i], "  <-----\n\n\tVERWIJDER EN VERVANG MET EEN NIEUWE EXPORT.\n\n")
    write.table(error_text, output_log, col.names = F, row.names = F)
    cat(error_text)
    df_raw <- NULL
    break
  }
}
rm(df_tmp)


# DATA PREPARATIE ---------------------------------------------------------
# Bepaal kleuren
kleuren <- data.frame(
  parameter = c("Chloride", "Bemonsteringsdiepte", "Geleidendheid", "Temperatuur"),
  kleur     = c("#4985de", "#b05435","#a81d9a","#d61604" ))

OMS_ID   <- "OMS22-906" # bepaal enige Order.number om te gebruiken

locs <- locs %>% 
  mutate(loc_oms_id = paste0(.$loc_oms, " (", .$Namespace_loc_id, ")")) %>% 
  arrange(Volgorde)

# Selecteer juiste data, combineer met hulpbestanden, voeg kolommen toe
df <- df_raw %>% 
  filter(Result.status == "Authorised") %>% # lege regels uitfilteren
  filter(!is.na(Result.text)) %>% # lege regels uitfilteren, zou dubbelop moeten zijn
  select(names(.)[names(.) %in% cols]) %>% # filter op gewenste kolommen
  filter(!duplicated(.)) %>% # Verwijder dubbele regels
  filter(Order.number %in% OMS_ID) %>%  # filter op gewenste Order.number
  filter(Wnsident %in% params$Wnsident, # filter obv precieze unit
         Component.name %in% params$Naam) %>% # filter obv precieze naam
  mutate(meting = as.numeric(Result.text)) %>% 
  select(-Result.text) %>% 
  left_join(locs, by = c("Sample.name" = "Namespace_loc_id")) %>% 
  left_join(params, by = c("Component.name" = "Naam", "Wnsident" = "Wnsident")) %>% 
  left_join(kleuren, by = c("Triviale_naam" = "parameter")) %>% 
  mutate(datum = date(mdy_hm(Sampled.date)), 
         text_y_axis = paste0(.$Triviale_naam, " (", .$Units, ")")) %>% 
  mutate(loc_oms_id = factor(loc_oms_id, levels = locs$loc_oms_id)) %>% 
  arrange(loc_oms_id, text_y_axis, datum)
  

# ANALYSE / CONTROLE ------------------------------------------------------
min_datum <- min(df$datum)
max_datum <- max(df$datum)

create_varplot <- function(df, VAR) {
  df_sel    <- df[df$Triviale_naam == VAR, ]
  
  y_as_naam <- unique(df_sel$text_y_axis)
  kleur     <- unique(df_sel$kleur)
  
  PLOT      <- ggplot(df_sel, aes(x = datum, y = meting)) +
    geom_line(color = kleur, size = 1) + geom_point(size = 2) + 
    theme_bw() + ylab(y_as_naam) + xlim(min_datum, max_datum) + #theme(axis.text.x=element_blank() ) +
    facet_wrap(~loc_oms_id, ncol = 4)
  
  return(PLOT)
}

# Combineer 4 grafieken
p1        <- create_varplot(df, params$Triviale_naam[1])
p2        <- create_varplot(df, params$Triviale_naam[2])
p3        <- create_varplot(df, params$Triviale_naam[3])
p4        <- create_varplot(df, params$Triviale_naam[4])


# WEGSCHRIJVEN RESULTATEN -------------------------------------------------
# Schrijf naar PDF
pdf(output_pdf, width = 16, height = 10)

all_plots <- grid.arrange(p1, p2, p3, p4, nrow = 4)
print(all_plots)

dev.off()

# Combineer kaart en grafieken pdf
pdf_combine(input = c("KAART_KWA_Meetpunten.pdf", output_pdf), output = output_pdf_en_kaart)




