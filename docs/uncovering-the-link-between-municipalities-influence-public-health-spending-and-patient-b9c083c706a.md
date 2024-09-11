# 揭示巴西市政影响、公共卫生支出和患者转移之间的关联

> 原文：[https://towardsdatascience.com/uncovering-the-link-between-municipalities-influence-public-health-spending-and-patient-b9c083c706a?source=collection_archive---------14-----------------------#2023-04-19](https://towardsdatascience.com/uncovering-the-link-between-municipalities-influence-public-health-spending-and-patient-b9c083c706a?source=collection_archive---------14-----------------------#2023-04-19)

## 一个引人入胜的故事旅程，与 Quarto、Shiny 和 ChatGPT 一同进行

[](https://fernandobarbalho.medium.com/?source=post_page-----b9c083c706a--------------------------------)[![Fernando Barbalho](../Images/0d145585cc73b89b4426af47b41844c5.png)](https://fernandobarbalho.medium.com/?source=post_page-----b9c083c706a--------------------------------)[](https://towardsdatascience.com/?source=post_page-----b9c083c706a--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----b9c083c706a--------------------------------) [Fernando Barbalho](https://fernandobarbalho.medium.com/?source=post_page-----b9c083c706a--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fdcaa8c7ce010&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Funcovering-the-link-between-municipalities-influence-public-health-spending-and-patient-b9c083c706a&user=Fernando+Barbalho&userId=dcaa8c7ce010&source=post_page-dcaa8c7ce010----b9c083c706a---------------------post_header-----------) 发布在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----b9c083c706a--------------------------------) ·11 min read·Apr 19, 2023[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fb9c083c706a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Funcovering-the-link-between-municipalities-influence-public-health-spending-and-patient-b9c083c706a&user=Fernando+Barbalho&userId=dcaa8c7ce010&source=-----b9c083c706a---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fb9c083c706a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Funcovering-the-link-between-municipalities-influence-public-health-spending-and-patient-b9c083c706a&source=-----b9c083c706a---------------------bookmark_footer-----------)![](../Images/f2431458930d7949b5dc0bab03859e93.png)

照片由 [Natanael Melchor](https://unsplash.com/fr/@nmelchorh?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 拍摄于 [Unsplash](https://unsplash.com/s/photos/Brazil-hospital?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

巴西公共卫生系统长期以来一直在努力提高资源分配和提供护理的效率。其中一个主要挑战是患者需要前往其他城市接受必要的医院治疗。根据巴西国家卫生系统的数据，我们估计仅在2021年，全国范围内的患者参与了大约400万次这样的旅行。本文探讨了一个处理这一公共卫生问题的项目的实施细节。阅读本文对于那些从事公共卫生政策工作的人尤其重要。此外，由于最终产品中使用了详细的代码，该文档也可能引起数据可视化和讲故事领域专业人士的兴趣。

为了更好地理解本文中的问题，我们调查了医院就诊支出与患者流动之间的关系。我们的分析揭示了巴西各城市之间支出存在显著不平等，小城市的支出远低于大城市。我们假设这种支出差异会导致从医院能力低的城市向医疗基础设施更为完善的城市流动的患者显著增加。

我们使用由巴西地理统计研究所（IBGE）制作的城市影响模型（REGIC）来验证我们的假设。我们证明了患者流动主要影响那些管理能力较弱、医院和门诊支出相对较少的小城市。同时，管理和影响能力较强的大城市更可能接收外来患者，从而增加了对其医疗服务的需求。

为了使我们的发现更易于广泛受众访问，我们开发了一个使用 Shiny、Quarto 和数据可视化技术的交互式仪表板。该仪表板允许用户实时探索数据，并通过动态的 ChatGPT 提示提供额外的见解和讲故事元素。通过利用这些工具，我们希望为巴西公共卫生系统面临的挑战提供新的视角，并为改进其绩效的持续努力做出贡献。请通过这个 [链接](https://siconfi-atendimento-hospitalar.tesouro.gov.br) 查看完整分析和交互页面。此外，以下部分展示了产品中使用的一些图表及相关代码。

## 公共卫生数据图表（和代码）

产品中使用的可视化重点关注了地图。目的是展示巴西城市在接受医院治疗时的旅行需求。下面的图表例如展示了不同城市在患者流向其他城市寻求医疗服务方面的差异。

![](../Images/d18ca5972e8644ba9e76071fe98d1a7f.png)

患者正在旅行寻求援助。图片由作者提供。

下面是构建数据集的代码块，这些数据集将作为使用ggplot绘制地图的基础。

```py
agrupamento_municipio<-
    dataset_analise %>%
      filter(
             deslocamento ==1) %>%
      group_by(munic_res) %>%
      summarise(
        numero_internacoes = n()
      ) %>%
      mutate(code_muni = munic_res,
             tipo_deslocamento = "saida" ) %>%
      bind_rows(
        dataset_analise %>%
          filter(
                 deslocamento ==1) %>%
          group_by(codufmun) %>%
          summarise(
            numero_internacoes = n()
          ) %>%
          mutate(code_muni = codufmun,
                 tipo_deslocamento = "entrada"),
        dataset_analise %>%
          filter(
                 deslocamento ==0) %>%
          group_by(codufmun) %>%
          summarise(
            numero_internacoes = n()
          ) %>%
          mutate(code_muni = codufmun,
                 tipo_deslocamento = "local")
      ) %>%
  group_by(code_muni, tipo_deslocamento) %>%
  summarise(
    total_internacoes = sum(numero_internacoes)
  )

agrupamento_municipio<-
agrupamento_municipio %>%
  tidyr::pivot_wider(names_from = tipo_deslocamento, values_from = total_internacoes) %>%
  mutate(liquido = ifelse(is.na(entrada),0,entrada)+
           ifelse(is.na(local),0,local)-
           ifelse(is.na(saida),0,saida))

agrupamento_municipio<-
agrupamento_municipio %>%
  mutate(local = ifelse(is.na(local),0,local),
         saida = ifelse(is.na(saida),0,saida),
         entrada = ifelse(is.na(entrada),0,entrada),
         perc_saida = saida/(saida+local)*100,
         perc_entrada = entrada/(entrada+local)*100,
         perc_entrada = ifelse(is.nan(perc_entrada),0,perc_entrada))

municipios_seat %>%
  mutate(code_muni = str_sub(as.character(code_muni),1,6)) %>%
  inner_join(agrupamento_municipio
  ) %>%
  inner_join(
    REGIC_trabalho%>%
      mutate(code_muni = str_sub(as.character(cod_cidade),1,6))
  ) %>%
  ggplot()+
  geom_sf(data = estados_mapa, fill=NA, color="#808080")+
  geom_sf(aes( fill= perc_saida),pch=21, color="#444444", size=2.9)+
  geom_text_repel(data = mun_sel_nivel_1A,aes(x=X, y=Y, label= name_muni),fontface = "bold", color="white")+
  geom_text_repel(data = mun_sel_nivel_1B,aes(x=X, y=Y, label= name_muni),fontface = "bold", color="white")+
  geom_text_repel(data = mun_sel_nivel_1C,aes(x=X, y=Y, label= name_muni),fontface = "bold", color="white")+
  geom_text_repel(data = mun_sel_nivel_2A,aes(x=X, y=Y, label= name_muni),fontface = "bold", color="white", force =2)+
  scale_fill_continuous_sequential(palette= "Heat 2")+
  labs(
    fill= str_wrap("% de pacientes internados em outros municípios",15) 
  )+
  theme_light() +
  theme(
    text = element_text(size=20),
    panel.background = element_rect(fill = "black"),
    panel.grid = element_blank(),
    axis.title.x = element_blank(),
    axis.title.y = element_blank(),
    strip.background = element_rect(fill = "#505050"),
    strip.text = element_text(color = "white"),
    axis.text = element_blank(),
    legend.key = element_rect(fill = "#15202B")

  )+
  facet_wrap(nome_nivel_hierarquia_ordenado~.)
```

地图上的每个点代表一个市镇，颜色表示该市的患者在其他市镇寻求护理的百分比。我们使用了“*facet*”数据可视化功能来展示每个城市在REGIC模型中的位置。在这个模型中，管理能力最高的最具影响力的城市是大都市（图中的第一行所示的组），而层级最低的是*地方中心*，位于最后一帧。

从地图上可以看出，*地方中心*集中在强烈的红色阴影点，这些点显示了需要旅行的患者高比例的市镇。另一方面，地图呈现了所有大都市的子层级，以黄色表示旅行需求较低。

图片强化了我们对极端层级在管理能力方面所赋予的自主权的预期。服务少且管理低时，对其他市镇的医院结构有强烈依赖。

指出REGIC模型中各级之间的流动是至关重要的。为此，我们使用汇流图。

![](../Images/6d2497a8027412e7817553c2bbf539a8.png)

REGIC级别之间的流动。作者提供的图片。

上面的汇流图是使用下面描述的代码构建的。在调用两个构建流图的函数之前，必须进行一些数据处理。

```py
ordem_y<- 
dataset_analise %>%
  filter(deslocamento==1,
         nome_nivel_hierarquia.x == "Centro Local",
         !(is.na(nome_nivel_hierarquia.y))) %>%
  group_by(nome_nivel_hierarquia.y) %>%
  summarise(
    quantidade = n()
  )  %>%
  ungroup() %>%
   inner_join(
     de_para_hierarquia %>%
       rename(nome_nivel_hierarquia.y=nome_nivel_hierarquia,
              entrada_abreviado = nome_abreviado)) %>%
  arrange(quantidade) %>%
  mutate(entrada = entrada_abreviado)

aluvial<-
     dataset_analise %>%
  filter(deslocamento==1,
         nome_nivel_hierarquia.x == "Centro Local",
         !(is.na(nome_nivel_hierarquia.y))) %>%
  mutate(saída = nome_nivel_hierarquia.x,
         entrada =nome_nivel_hierarquia.y ) %>%
  select(saída, entrada)

aluvial<- 
   aluvial %>%
   inner_join(
     de_para_hierarquia %>%
       rename(saída=nome_nivel_hierarquia,
              saida_abreviado = nome_abreviado)) %>%
   inner_join(
     de_para_hierarquia %>%
       rename(entrada=nome_nivel_hierarquia,
              entrada_abreviado = nome_abreviado)) %>%
   select(saida_abreviado, entrada_abreviado ) %>%
   rename(saída= saida_abreviado,
          entrada = entrada_abreviado)

 aluvial$entrada <- factor(aluvial$entrada, levels = unique(ordem_y$entrada[order(ordem_y$quantidade)])) 

 p<-
 alluvial_wide( data = aluvial, 
                max_variables = 2, 
                fill_by = 'first_variable') 

parcats::parcats(p, data_input = aluvial,marginal_histograms = FALSE,labelfont = list(size = 15, color = "black"), sortpaths= "backwards")
```

上图所示的流动显示了来自被归类为地方中心的市镇的患者主要按以下顺序流动：次区域中心B（17.6%）、区域首府C（16.6%）、次区域中心A（15.7%），仅在第四位的是大都市（13.8%）。由此可见，当患者需要医院护理时，REGIC等级在某种程度上是被攀升的。大都市对地方中心有吸引力，但其他城市层级的接近和管理能力调节了这一点。

我们发现一些城市在接收来自其他城市的患者方面具有重要意义。因此，我们制作了两张地图，展示了两个在接收患者方面突出的城市的旅行影响，显示了旅行距离和接收的患者数量。为此，请查看下面的地图，重点关注累西腓，这个巴西城市接收了最多的其他地点的患者。

![](../Images/3f7334fa539a8e4aa6246539953d5992.png)

患者前往累西腓。作者提供的图片。

下面的代码稍长。在创建两个图形的两个对象之前，需要进行大量的数据转换。这里我们使用*{patchwork}*包将图表并排放置。

```py
municipio_selecionado<-"261160"
muni_sel<- 
  dataset_analise %>%
  filter(deslocamento ==1,
         codufmun== municipio_selecionado) %>%
  group_by(codufmun,nome_nivel_hierarquia_ordenado.y, uf.y) %>%
  summarise(quantidade = n()) %>%
  rename(code_muni= codufmun,
         hierarquia = nome_nivel_hierarquia_ordenado.y,
         uf = uf.y) %>%
  mutate(tipo_deslocamento  = "destino",
         distancia = 0) %>%
  bind_rows(
    dataset_analise %>%
      filter(deslocamento ==1,
             codufmun== municipio_selecionado) %>%
      group_by(munic_res,nome_nivel_hierarquia_ordenado.x, uf.x) %>%
      summarise(
        quantidade = n(),
        distancia =min(distancia)
      ) %>%
      ungroup() %>%
      rename(code_muni= munic_res,
             hierarquia = nome_nivel_hierarquia_ordenado.x,
             uf=uf.x)%>%
      mutate(tipo_deslocamento  = "origem")
  )

muni_sel_posicao<-
dataset_analise %>%
dplyr::filter(deslocamento ==1,
        codufmun==  municipio_selecionado)%>%
  distinct(codufmun, mun_res_lat.x, mun_res_lat.y, mun_res_lon.x, mun_res_lon.y,distancia)

muni_sel_posicao<-
  municipios_seat %>%
  mutate(code_muni = str_sub(as.character(code_muni),1,6)) %>%
  inner_join(
    muni_sel_posicao %>%
      rename(code_muni= codufmun)
  )

muni_sel_repel<-
   municipios_seat %>%
   mutate(code_muni = str_sub(as.character(code_muni),1,6)) %>%
   filter(code_muni %in% c("260960", "260790","120020")) %>% #261160-Recife,260790 -Jaboatão, 260960 - Olinda, 260410 - Caruarau, 260545 - Fernando de Noronha, 120020 - Cruzeiro do Sul-AC
  inner_join(muni_sel)

xmin<- min(min(muni_sel_posicao$mun_res_lon.x), min(muni_sel_posicao$mun_res_lon.y)) -1
xmax <- max(max(muni_sel_posicao$mun_res_lon.x), max(muni_sel_posicao$mun_res_lon.y)) +1

ymin<- min(min(muni_sel_posicao$mun_res_lat.x), min(muni_sel_posicao$mun_res_lat.y)) -1
ymax <- max(max(muni_sel_posicao$mun_res_lat.x), max(muni_sel_posicao$mun_res_lat.y)) +1

g1<-
municipios_seat %>%
  mutate(code_muni = str_sub(as.character(code_muni),1,6)) %>%
  inner_join(
    muni_sel
  ) %>%
  ggplot()+
  geom_sf(data = estados_mapa, fill=NA, color="#505050")+
  geom_curve(data=muni_sel_posicao, aes(x=mun_res_lon.x,y=mun_res_lat.x,xend=mun_res_lon.y,yend=mun_res_lat.y, colour= distancia),
             curvature = -.25, ncp = 800,size = 1)+
  geom_sf(fill="white",size=1.9,pch=21, color="#444444")+
  scale_fill_discrete_qualitative(palette="dark2")+
  scale_color_continuous_sequential(palette= "Heat 2")+
  coord_sf(xlim = c(xmin,xmax), ylim=c(ymin,ymax))+
  labs(
    fill= "",
    color = str_wrap("distância em Km",10)
  )+
  theme_light() +
  theme(
    text = element_text(size=18),
    panel.background = element_rect(fill = "black"),
    panel.grid = element_blank(),
    axis.title.x = element_blank(),
    axis.title.y = element_blank(),
    strip.background = element_rect(fill = "#505050"),
    strip.text = element_text(color = "white"),
    axis.text = element_blank(),
  ) 
muni_sel_foco<-
  municipios_seat %>%
  mutate(code_muni = str_sub(as.character(code_muni),1,6)) %>%
  inner_join(
      muni_sel%>%
  filter(code_muni==municipio_selecionado)
  )

 muni_sel<-
   muni_sel%>%
   filter(code_muni!=municipio_selecionado)

set.seed(1972)
g2<-
municipios_seat %>%
  mutate(code_muni = str_sub(as.character(code_muni),1,6)) %>%
  inner_join(
    muni_sel
  ) %>%
  ggplot()+
  geom_sf(data = estados_mapa, fill=NA, color="#505050")+#505050
  geom_sf( aes(fill=quantidade),pch=21, color="#444444", size=2, show.legend = TRUE)+
  geom_sf( data= muni_sel_foco, aes(size=quantidade),pch=21, color="#444444", fill="white")+
  geom_text_repel(data = muni_sel_repel,
                  aes(x=X, y=Y, label= str_wrap(paste(name_muni,":",quantidade),10)), 
                  color = "white", 
                  limits = c(0,2352),
                  fontface = "bold", 
                  nudge_x = c(0,2,2.5), 
                  nudge_y = c(0,-3.5,2),
                  show.legend = TRUE)+
  geom_text_repel(data = muni_sel_foco,
                  aes(x=X, y=Y, label= str_wrap(name_muni,20)),
                  fontface = "bold", 
                  color="white",
                  nudge_x = c(3), 
                  nudge_y = c(0))+
  scale_fill_continuous_sequential(palette= "Heat", trans= "log2" )+
  coord_sf(xlim = c(xmin,xmax), ylim=c(ymin,ymax))+
  labs(

    fill = str_wrap("Quantidade de saídas",15),
    size= str_wrap("Quantidade de entradas",15)
  )+
  theme_light() +
  theme(
    text = element_text(size=18),
    panel.background = element_rect(fill = "black"),
    panel.grid = element_blank(),
    axis.title.x = element_blank(),
    axis.title.y = element_blank(),
    strip.background = element_rect(fill = "#505050"),
    strip.text = element_text(color = "white"),
    axis.text = element_blank(),
    legend.key = element_rect(fill = "#15202B")
  )

library(patchwork)
g1|g2
```

当涉及到医院护理时，可以看到大城市累西腓在整个巴西的影响。通过观察左侧地图上的哈弗斯距离，可以看到累西腓为距离超过 4000 公里之外的患者提供服务。样本表明，佩鲁南布科的首府接收了几乎来自全国所有联邦单位的患者。另一方面，当评估右侧地图时，可以发现最显著的影响发生在大都市区的城市，特别是奥林达和贾博瓦豆斯·瓜拉雷佩斯。同时，还可以看到在佩鲁南布科州整个区域延伸的红色阴影点。还可以识别对邻近州的影响，特别是帕拉伊巴、阿拉戈斯和里约格朗德 do 诺特。

在我们原始叙述的最后，我们需要展示较低的市级医院护理支出与患者转移需求较大之间的关联。为了测试这种关联，我们创建了来自 5570 个巴西市镇的患者流失和接收百分比的聚类。利用 PAM 技术生成的轮廓系数，我们确定了四个组：中等进入、弱退出、中等退出和强退出。最后一个组对分析最为重要。以下图表提供了评估组重要性的见解。

![](../Images/b0b3ec064d37f0b0ca8b403d8a6f9899.png)

医院护理费用的箱线图。作者提供的图片。

下面的代码首先将数据加载到内存中，数据包括市镇的聚类。接下来，使用 ggplot，我们利用 {patchwork} 包构建了两个并排放置的箱线图。

```py
agrupamento_municipio_cluster<-readRDS("agrupamento_municipio_2021.RDS")

g1<-
dataset_analise %>%
  filter(deslocamento == 1,
         perc.x>0,
         perc.x<=50) %>% 
  distinct(nome_nivel_hierarquia.x,munic_res, perc.x) %>%
  inner_join(
    agrupamento_municipio_cluster %>%
      rename(munic_res=code_muni)
  ) %>%
  ggplot() +
  geom_jitter(aes(x=cluster_4_k, y=perc.x, fill=perc_saida), pch=21, color="#444444",size=2)+
  geom_boxplot(aes(x=cluster_4_k, y=perc.x),fill=NA, color= "white", outlier.shape = NA)+
  scale_fill_continuous_sequential(palette= "Red-Yellow")+
  theme_light() +
  theme(
    text = element_text(size=18),
    panel.background = element_rect(fill = "black"),
    panel.grid = element_blank(),
    axis.title.x = element_blank(),
    strip.background = element_rect(fill = "#505050"),
    strip.text = element_text(color = "white"),
    #axis.text = element_blank(),
    axis.text.x = element_text(angle = 45, vjust = 0.5),
    legend.key = element_rect(fill = "#15202B")
  )+
  labs(
    fill= "(%) saída",
    y = "Gastos Hospitalares e ambulatoriais - (%) do total"
  )

g2<-
dataset_analise %>%
  filter(deslocamento == 1,
         perc.y>0,
         perc.y<=50) %>%
  distinct(nome_nivel_hierarquia.y,codufmun, perc.y) %>%
  inner_join(
    agrupamento_municipio_cluster %>%
      rename(codufmun=code_muni)
  ) %>%
  ggplot() +
  geom_jitter(aes(x=cluster_4_k, y=perc.y, fill=perc_entrada), pch=21, color="#444444",size=2)+
  geom_boxplot(aes(x=cluster_4_k, y=perc.y),fill=NA, color= "white", outlier.shape = NA)+
  scale_fill_continuous_sequential(palette= "Red-Yellow")+
  theme_light() +
  theme(
    text = element_text(size=18),
    panel.background = element_rect(fill = "black"),
    panel.grid = element_blank(),
    axis.title.x = element_blank(),
    strip.background = element_rect(fill = "#505050"),
    strip.text = element_text(color = "white"),
    axis.text.x = element_text(angle = 45, vjust = 0.5),
    legend.key = element_rect(fill = "#15202B")
  )+
  labs(
    fill= "(%) entrada",
    y = "Gastos Hospitalares e ambulatoriais - (%) do total"
  )
g1|g2
```

图中的每一个彩色点代表一个巴西市镇。点的颜色表示左侧图中的患者流出百分比和右侧图中来自其他城市患者的出席百分比。在两个图的横轴上，我们可以看到分组，纵轴上则是住院和门诊护理的费用百分比。

通过观察图表，我们可以看到费用与患者流入和流出组之间关联的最重要结论。当我们分析左侧图表中的患者流出时，我们发现*强退出*组的医院费用中位数远低于其他组。

## 你是否询问了具有互动性的 Shiny？

正如我们在文本开头所示，除了主要故事外，我们还准备了多个选项卡，允许用户进行过滤并生成探索互动中指定城市现实的图表。请参见下方这些互动的一些截图。注意几乎所有选项卡中都有下载与图表相关的数据的选项。

![](../Images/87b46f0032360be5839f1d65c8154fea.png)

患者流动 — 巴西。作者提供的图片。

![](../Images/dd9445ee5f3534f2c4d53a8cd16c3b20.png)

患者流向所选城市。图片由作者提供。

![](../Images/88758477cdd2646e9da2b99e01c9300c.png)

选定城市在箱线图中的位置。图片由作者提供。

一个选项卡显示了可以用于过滤和下载的数据的完整表格。

![](../Images/dbf1a5de62853b2827b25b99e163d367.png)

一张展示市政当局的X光图。图片由作者提供。

## ChatGPT怎么样？

最后一张选项卡显示了用户所选市政当局的主要信息汇总数据。请见下方屏幕截图。

![](../Images/79e5bf540cf61b2ee7231abec8cd1388.png)

信息摘要和生成ChatGPT的提示。图片由作者提供。

最后一张表格是可以与ChatGPT进行交互的地方。面板动态生成一个包含其他表格数据的提示。用户可以按下复制按钮，将提示带到ChatGPT，观察神奇的效果。查看一个示例的截图。（如果读者不懂葡萄牙语并想了解提示和AI的回应，请通过电子邮件联系我：[fbarbalho@gmail.com](mailto:fbarbalho@gmail.com)）。

![](../Images/aa4f2d4c301753d001069295c4af8bc4.png)

由应用程序生成的提示。图片由作者提供。

![](../Images/50c873c4a6a0ec495d0ce0145a1a2759.png)

ChatGPT生成的文本作为对应用程序生成的提示的回应。图片由作者提供。

## 代码和数据

完整代码可以在[github](https://github.com/tchiluanda/siconfi_atendimento_hospitalar)找到。

所有数据集都被归类为公共领域，因为这些数据是由巴西联邦政府机构生产的，作为主动透明性在互联网公布，并且受巴西信息获取法的管辖。

+   [Datasus](https://datasus.saude.gov.br/transferencia-de-arquivos/): 关于健康设施地址和医院就诊的数据。

+   [IBGE](https://www.ibge.gov.br/#): 有关REGIC模型的数据。

+   [国家财政部](https://siconfi.tesouro.gov.br/siconfi/index.jsf;jsessionid=U5bso6l-RBmLg5-OdG7y9b4A.node2): 有关健康支出的数据。

作者感谢[Ben Huberman](https://benzbox.medium.com/)的宝贵评论。
