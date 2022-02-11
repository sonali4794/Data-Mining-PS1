## R Markdown

Q2\] Billboard Top 100

    library("tidyverse")
    library("dplyr")
    library("magrittr")
    library("tidyverse")
    library("dplyr")
    library("magrittr")

    billboard = read.csv("C:/Users/hp/Downloads/billboard.csv")

    top10 = billboard %>%
      group_by(song, performer) %>%
      summarise(count = n()) %>%
      arrange(desc(count)) %>%
      head(10) %>%
      select(song, performer, count)
    top10

    ## # A tibble: 10 x 3
    ## # Groups:   song [10]
    ##    song                             performer                            count
    ##    <chr>                            <chr>                                <int>
    ##  1 Radioactive                      Imagine Dragons                         87
    ##  2 Sail                             AWOLNATION                              79
    ##  3 Blinding Lights                  The Weeknd                              76
    ##  4 I'm Yours                        Jason Mraz                              76
    ##  5 How Do I Live                    LeAnn Rimes                             69
    ##  6 Counting Stars                   OneRepublic                             68
    ##  7 Party Rock Anthem                LMFAO Featuring Lauren Bennett & Go~    68
    ##  8 Foolish Games/You Were Meant Fo~ Jewel                                   65
    ##  9 Rolling In The Deep              Adele                                   65
    ## 10 Before He Cheats                 Carrie Underwood                        64

    div = billboard %>%
      filter(year != "1958" & year != "2021") %>%
      group_by(year, song) %>%
      summarise(count = n())

    diverse = div %>%
      group_by(year) %>%
      summarise(no_of_unique_songs = n())

    ggplot(diverse) +
      geom_line(aes(x = year, y = no_of_unique_songs)) +
      labs(x = "Year", y = "No of Songs", title = "Musical Diversity", caption = "The graph displays how the number of unique songs has change from 1959 to 2020. It appears that around 1970 there were many songs that made to the billboard top 100. Whereas the billboard saw least number of songs around 2001-2002. This can be a good data point to study the creativity of music composers as well the taste of audience")

![](PSV1_files/figure-markdown_strict/unnamed-chunk-1-1.png)

    twhit = billboard %>%
      group_by(performer, song) %>%
      summarise(n1 = n()) %>%
      filter(n1 >= 10)

    tenweekhit = twhit %>%
      group_by(performer) %>%
      summarise(n2 = n()) %>%
      arrange(desc(n2)) %>%
      head(19)

    ggplot(tenweekhit) +
      geom_col(aes(x = fct_reorder(performer, n2), y = n2), color = "red", fill = "red") +
      coord_flip() +
      labs(x = "Artists", y = "No of 10-week hit Songs",
           caption = "The graph shows list of top 19 artists who have had at least one song apper in the top 100 billboard. Elton outranks all with a high margin")

![](PSV1_files/figure-markdown_strict/unnamed-chunk-1-2.png)

Q3\] Olympics Wrangling

    library("tidyverse")
    library("dplyr")
    library("magrittr")

    olympics_top20 = read.csv("C:/Users/hp/Downloads/olympics_top20.csv")
    view(olympics_top20)

    df1 = olympics_top20 %>%
      filter(sport == "Athletics" & sex == "F") %>%
      group_by(name) %>%
      summarise(ht = mean(height))

    q95 = quantile(df1$ht, 0.95)
    q95

    ## 95% 
    ## 183

    print("The 95th percentile height of female athletes is 183")

    ## [1] "The 95th percentile height of female athletes is 183"

    df2 = olympics_top20 %>%
      filter(sex == "F") %>%
      group_by(event) %>%
      summarise(variation = sd(height)) %>%
      arrange(desc(variation))

    maxsd = max(df2$variation, na.rm = TRUE)
    maxsd

    ## [1] 10.86549

    print("Highest variation (measured in terms of std dev) is in Rowing Women's Coxed Fours event followed by Women's basketball")

    ## [1] "Highest variation (measured in terms of std dev) is in Rowing Women's Coxed Fours event followed by Women's basketball"

    df_male = olympics_top20 %>%
      filter(sport == "Swimming") %>%
      filter(sex == "M") %>%
      group_by(year) %>%
      summarise(agem = mean(age))%>%
      select(year, agem)

    df_female = olympics_top20 %>%
      filter(sport == "Swimming") %>%
      filter(sex == "F") %>%
      group_by(year) %>%
      summarise(agef = mean(age))%>%
      select(year, agef)

    df_all = olympics_top20 %>%
      filter(sport == "Swimming") %>%
      group_by(year) %>%
      summarise(ageall = mean(age)) %>%
      select(year, ageall)

    dfm1 = merge(df_all, df_female, by.x = "year", by.y = "year", all = TRUE)
    df = merge(dfm1, df_male, by.x = "year", by.y = "year", all = TRUE)
    df[is.na(df)] = 0

    ggplot(df)+
      geom_line(aes(x = year, y = ageall, color = "all"))+
      geom_line(aes(x = year, y = agem, color = 'male'))+
      geom_line(aes(x = year, y = agef, color = 'female'))+
      labs(x = "Years when Swimming event was held at Olympics", y = "Average Age of Swimmers", caption = "", color = "sex") +
      ylim(0, 40)

![](PSV1_files/figure-markdown_strict/unnamed-chunk-2-1.png)

Q4\]K-nearest Neighhbours

    library(tidyverse)
    library(ggplot2)
    library(rsample) 
    library(caret)
    library(modelr)
    library(parallel)
    library(foreach)

    sclass = read_csv('sclass.csv')

    data1 = sclass %>% 
      filter(trim == '350')

    data1_split =  initial_split(data1, prop=0.9)
    data1_train = training(data1_split)
    data1_test  = testing(data1_split)

    k1_rmse = foreach(k = 2:200, .combine='rbind') %do% {
      knn = knnreg(price ~ mileage, data=data1_train, k=k)
      rms = rmse(knn, data1_test)
      c(k=k, e=rms)
    } %>% as.data.frame

    ggplot(k1_rmse) + 
      geom_point(aes(x=k, y=e)) + 
      labs(x = 'Varying values of K',y = 'RMSE', title = 'K vs RMSE for 350 Model ') +
      scale_x_log10() 

![](PSV1_files/figure-markdown_strict/unnamed-chunk-3-1.png)

    optimal_k1 = k1_rmse%>%
      filter(e == min(e))
    optimal_k1

    ##            k        e
    ## result.14 15 10126.09

    knn_1 = knnreg(price ~ mileage, data = data1_train, k = optimal_k1)
    data1_test = data1_test %>%
      mutate(price_pred = predict(knn, data1_test))

    ggplot(data = data1_test) +
      geom_point(aes(x = mileage, y = price)) +
      geom_line(aes(x = mileage, y = price_pred), color = 'red')+
      labs(title = "Fitted model for 350 Model")

![](PSV1_files/figure-markdown_strict/unnamed-chunk-3-2.png)

    data2 = sclass %>% 
      filter(trim == '63 AMG')

    data2_split =  initial_split(data2, prop=0.9)
    data2_train = training(data2_split)
    data2_test  = testing(data2_split)

    k2_rmse = foreach(k = 2:200, .combine='rbind') %do% {
      knn = knnreg(price ~ mileage, data=data2_train, k=k)
      rms = rmse(knn, data2_test)
      c(k=k, e=rms)
    } %>% as.data.frame

    ggplot(k2_rmse) + 
      geom_point(aes(x=k, y=e)) + 
      labs(x = 'Varying values of K',y = 'RMSE', title = 'K vs RMSE for 63 AMG Model ') +
      scale_x_log10() 

![](PSV1_files/figure-markdown_strict/unnamed-chunk-3-3.png)

    optimal_k2 = k2_rmse%>%
      filter(e == min(e))
    optimal_k2

    ##            k        e
    ## result.55 56 13244.99

    knn_2 = knnreg(price ~ mileage, data = data2_train, k = optimal_k2)
    data2_test = data2_test %>%
      mutate(price_pred = predict(knn, data2_test))


    ggplot(data = data2_test) +
      geom_point(aes(x = mileage, y = price)) +
      geom_line(aes(x = mileage, y = price_pred), color = 'red')+
      labs(title = "Fitted model for 63 AMG Model")

![](PSV1_files/figure-markdown_strict/unnamed-chunk-3-4.png)
