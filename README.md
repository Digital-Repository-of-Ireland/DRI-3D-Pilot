## DRI 3D Pilot

The DRI 3D Pilot is a project headed by the [Digital Repository of Ireland](https://www.dri.ie/) (DRI) to investigate ways to support Cultural Heritage Institutions in creating, publishing and aggregating 3D content to Europeana Collections, via the DRI, the Europeana National Aggregator for Ireland. The project is part-funded by the European Union within the Europeana Common Culture project.

### Introduction

The purpose of this project was to research and develop workflows and tools for the preparation, ingest and aggregation of 3D Content.

### Review of 3D Content in Europeana

The 3D pilot set out to investigate methods of preparing and aggregating 3D content to Europeana. This work began with a [Review of 3D material in Europeana](https://github.com/Digital-Repository-of-Ireland/DRI-3D-Pilot/blob/master/Report%20on%203D%20Content%20in%20Europeana.pdf).

At the time of conducting this review, there were 28,573 records in Europeana Collections tagged as 3D. A large portion of these, however, had broken links and have subsequently been removed from Europeana Collections search results, or were simply mistagged as 3D content. Of the remainder, the most popular format was 3DPDF. This format, although formerly popular, is now poorly supported on most platforms.

The next most common content consisted of a variety of embedded 3D viewers. Of these, the majority used the 3Dsom viewer, but all of these used the older flash version of this viewer. More recently aggregated content tended to use the Sketchfab viewer, with a smaller number using various proprietary viewers, or offering only panoramic photographs rather than true 3D models.

Only a small proportion of these provided the option to download the model for reuse, even where the license allowed reuse.

### 3D Content in Europeana Task Force

The project collaborated closely with the 3D Content in [Europeana Task Force](https://pro.europeana.eu/project/3d-content-in-europeana). The initial review formed an input to the Task force's [Final Report](https://pro.europeana.eu/files/Europeana_Professional/Europeana_Network/Europeana_Network_Task_Forces/Final_reports/3D-TF-final%20report.pdf). Other input was provided to the Task Force activities, and the project undertook to implement the recommendations of the Task Force.

### Preprocessing workflows for 3D content

We then investigated the types of 3D content held by Irish Cultural Heritage Institutions. It was found that these were a mixture of large point clouds, meshes and models in a variety of file formats. To allow the pilot application to have the most impact we first explored workflows and tools that could be used to process these various file types into formats suitable for aggregation to Europeana.

Larger point cloud files cannot be displayed as they are in Europeana or other web-based platforms, but must first have a lightweight mesh generated from the point cloud. We set out to compare a variety of tools to achieve this. The tools included open source tools which could semi-automate the mesh reconstruction process such as CloudCompare and meshlab, proprietary software such as RealityCapture, as well as open source libraries such as open3D which could be used to develop custom mesh reconstruction code.

Developing custom code gave the benefit of allowing integration with the existing platform and a simpler more automated workflow for aggregation to Europeana.  The quality of the resultant meshes was lower than that obtained by the semi-automated or manual process using other tools, however, and did not produce models that were of sufficient quality for aggregation. We are in the process of writing up these results as recommendations to content providers.

Where the content came in the format of meshes or models this pre-processing step was generally not necessary, although the quality varied by content provider. More detail on the workflows involved were published in [Europeana Tech Insight issue 14](https://pro.europeana.eu/page/issue-14-3d)

### Viewers for 3D Content

Based on the recommendations of the 3D Task Force we looked into various options for how to aggregate these models to Europeana as EDM. Many of the Europeana content providers currently aggregating 3D content choose to host their models on third-party platforms. While this is suitable for smaller organisations without the capacity to host large volumes of data, it builds in reliance on a service which is out of control of the Content Providers and Aggregators, and there can be issues with the cost of such services as well as the licencing and export or access methods.

As Sketchfab is a very popular option within the Europeana community, we investigated this, but the fees involved make this a less attractive option. For this pilot we eventually opted for a self-hosted 3D viewer as part of the National Aggregator platform. We looked into three possible solutions

- 3DHop
- Smithsonian VOyager
- Three Javascript Library

Of these 3DHop and Smithsonian Voyager only had limited support for input file formats. This was a problem for the pilot as the content was in such a wide variety of formats. It was decided to use the Three JavaScript Library to display these models online. This is now being integrated with the DRI Repository platform and the Europeana aggregation feed.

Once completed it will be possible to aggregate the models to Europeana by putting the source file url into the edm:isShownBy element in EDM. These source files are rarely, however, renderable within a web browser, and thus they would not provide a very satisfactory user experience.

In order to ensure that the 3D object can be rendered and interacted with in a browser environment, an embedded version would be more appropriate.

The [Europeana Publishing Guide](https://pro.europeana.eu/post/publication-policy) indicates that any oEmbed compliant player is supported by Europeana Collections. We are thus investigating an oEmbed option which would allow us to send an embeddable 3D model in the edm:isShownBy field of EDM. This may be accompanied by an additional edm:hasView element which gives the url to download the source file, or a link to the source file may be provided in the embeddable viewer.

### 3D Content Providers

DRI is currently working with [Transport Infrastructure Ireland](https://www.tii.ie/) who are providing content for the 3D Pilot. We are interested in working with other Irish institutes who hold 3D content. More information on joining DRI can be found at our [members pages](https://www.dri.ie/membership).
