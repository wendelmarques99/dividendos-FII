library(magrittr)

lista_de_fiis <- read.csv("datafiis.csv") %>% 
  janitor::clean_names()

# Montando a funcao -------------------------------------------------------
get_dividends <- function(papel){
  ticker <- papel
  url <- glue::glue("https://www.fundamentus.com.br/fii_proventos.php?papel={ticker}&tipo=2")
  rvest::read_html(url) %>% 
    rvest::html_table() %>% 
    purrr::pluck(1) %>% 
    janitor::clean_names() %>% 
    dplyr::mutate(valor = as.numeric(gsub(pattern = "\\,", "\\.", x = valor)), 
                  ultima_data_com = lubridate::dmy(ultima_data_com), 
                  data_de_pagamento = lubridate::dmy(data_de_pagamento), 
                  ticker = ticker) %>% 
    # pegando a ultima data de pagamento
    dplyr::filter(data_de_pagamento  == head(data_de_pagamento , 1))
}

# Rodando a funcao para os FIIs -------------------------------------------

dados_output <- purrr::map_dfr(.x = lista_de_fiis$i_ativo, .f = get_dividends)

# exportar em excel
rio::export(dados_output, "dividendos_FII.xlsx")


# tipo de mercado nota logn falar com felipe