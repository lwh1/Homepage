---
title: "Enabling DNN Acceleration with Data and Model Parallelization over Ubiquitous End Devices"

# Authors
# If you created a profile for a user (e.g. the default `admin` user), write the username (folder name) here 
# and it will be replaced with their full name and linked to their profile.
authors:
- Yakun Huang
- Xiuquan Qiao
- Wenhai Lai
- Schahram Dustda
- Jianwei Zhang
- Jiulin Li

# Author notes (optional)
# author_notes:
# - "Equal contribution"
# - "Equal contribution"

date: "2021-07-01T00:00:00Z"
doi: ""

# Schedule page publish date (NOT publication's date).
publishDate: "2021-07-01T00:00:00Z"

# Publication type.
# Legend: 0 = Uncategorized; 1 = Conference paper; 2 = Journal article;
# 3 = Preprint / Working Paper; 4 = Report; 5 = Book; 6 = Book section;
# 7 = Thesis; 8 = Patent
publication_types: ["2"]

# Publication name and optional abbreviated publication name.
publication: In *IEEE Internet Of Things Journal*
publication_short: In *JIOT*

abstract: Deep neural network (DNN) shows great promise in providing more intelligence to ubiquitous end devices. However, the existing partition-offloading schemes adopt data-parallel or model-parallel collaboration between devices and the cloud, which does not make full use of the resources of end devices for deep-level parallel execution. This paper proposes eDDNN (i.e. enabling Distributed DNN), a collaborative inference scheme over heterogeneous end devices using cross-platform web technology, moving the computation close to ubiquitous end devices, improving resource utilization, and reducing the computing pressure of data centers. eDDNN implements D2D communication and collaborative inference among heterogeneous end devices with WebRTC protocol, divides the data and corresponding DNN model into pieces simultaneously, and then executes inference almost independently by establishing a layer dependency table. Besides, eDDNN provides a dynamic allocation algorithm based on deep reinforcement learning to minimize latency. We conduct experiments on various datasets and DNNs and further employ eDDNN into a mobile web AR application to illustrate the effectiveness. The results show that eDDNN can achieve the latency decrease by 2.98x, reduce mobile energy by 1.8x, and relieve the computing pressure of the edge server by 2.57x, against a typical partition-offloading approach.

# Summary. An optional shortened abstract.
summary: This paper proposes eDDNN (i.e. enabling Distributed DNN), a collaborative inference scheme over heterogeneous end devices using cross-platform web technology, moving the computation close to ubiquitous end devices, improving resource utilization, and reducing the computing pressure of data centers. 

tags: []

# Display this page in the Featured widget?
featured: false

# Custom links (uncomment lines below)
# links:
# - name: Custom Link
#   url: http://example.org

url_pdf: 'https://ieeexplore.ieee.org/document/9538819'
url_code: ''
url_dataset: ''
url_poster: ''
url_project: ''
url_slides: ''
url_source: ''
url_video: ''
---

<!-- {{% callout note %}}
Click the *Cite* button above to demo the feature to enable visitors to import publication metadata into their reference management software.
{{% /callout %}}

{{% callout note %}}
Create your slides in Markdown - click the *Slides* button to check out the example.
{{% /callout %}} -->

