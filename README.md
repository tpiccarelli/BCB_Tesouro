# BCB_Tesouro

  titulos_publicos_BCB <- function(ano1, ano2){
    
    # formato anos 1 e 2: AAAA
    # LEMBRAR DE ALTERAR DIRETÓRIO DE TRABALHO (setwd())
    
    # Carrega packages:
    library(data.table)
    library(dplyr)
    library(xlsx)
    
    # Estrutura endereço do link:
    end <- "http://www4.bcb.gov.br/pom/demab/negociacoes/download/NegT"
    
    # Nomeia arquivo baixado:
    filename <- "download.zip"
    
    # Cria data.frame para consolidação dos dados baixados:
    df_consolid <- data.frame()
    
    for (i in (ano1:ano2)){
    
            for (j in (1:12)){
            
            # Exibe ano para cada ciclo da rodada:
            show(i)
            
            # Baixa arquivos mensais *.CSV:
            if (j <10)
                try(expr = {
                    download.file(paste0(end, i, 0, j, ".ZIP"), destfile = filename, mode = "wb", quiet = T)
                }, silent = T)
                
                else
                    try(expr = {
                        download.file(paste0(end, i, j, ".ZIP"), destfile = filename, mode = "wb", quiet = T)
                    }, silent = T)
            
            # Descompacta arquivo:
            unzip(zipfile = filename)
            
            # Remove arquivo "*.zip":
            file.remove(filename)
            
            # Lê arquivo *.csv baixado, lançando os dados em dataframe:
            df <- read.csv2(file = dir())
            df_consolid <- rbind(df_consolid, df)
            
            # Exclui arquivos *.CSV:
            file.remove(list.files(pattern = ".CSV"))
        }
    }
    
    df_consolid <<- df_consolid       
    
    # Salva arquivo .RData ao final da rodada, de modo a salvar os objetos gerados:
    save.image(file = "J:/GERIC 3/Títulos Públicos (BCB)/base_histórica.RData")
  }
