# मशीन लर्निंग के लिए क्लस्टरिंग मॉडल

क्लस्टरिंग (Clustering) एक ऐसा मशीन लर्निंग वर्ग है जो एक प्रकार के ऑब्जेक्ट्स को पहचान कर, उन्हें साथ संगृहीत (groups) करता है। एक प्रकार के संग्रहण को एक क्लस्टर (Cluster) बुलाया जाता ह।  क्लस्टरिंग की बाकि मशीन लर्निंग मॉडल से खासियत ये है की इसमें यह संग्रहण की प्रक्रिया स्वचालित है। यह सुपेर्विसेड लर्निंग से बिलकुल विपरीत है।  

## क्षेत्रीय विषय: नाइजीरियन लोगो के संगीत की पसंद को समझने के लिए क्लस्टरिंग का प्रयोग  🎧

नाइजीरिया के विभन्न लोगो की संगीत में विभन्न रूचि है।  सॉप्टीफाय से लिए हुए डाटा ([इस आर्टिकल से प्रेरित](https://towardsdatascience.com/country-wise-visual-analysis-of-music-taste-using-spotify-api-seaborn-in-python-77f5b749b421)) से, आइये नाइजीरिया के प्रसिद्ध संगीत के बारे मै जानते है। यह डाटा में अनेक संगीत की 'नृत्य योग्यता' (danceability score), 'ध्वनिकता' (acousticness), 'प्रबलता' (loudness), 'वाक्पटुता' (speechiness), 'लोकप्रियता' (popularity) और 'ऊर्जा' (energy) मौजूद है। इस डेटा में पैटर्न खोजना दिलचस्प होगा।

![एक टर्नटेबल](../images/turntable.jpg)

> <a href="https://unsplash.com/@marcelalaskoski?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText">मार्सेला लास्कोस्की (Marcela Laskoski)</a> के द्वारा ली गयी <a href="https://unsplash.com/s/photos/nigerian-music?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText">उनस्प्लैश (Unsplash)</a> पर एक तस्वीर
  
पाठों की इस श्रृंखला में, आप क्लस्टरिंग तकनीकों का उपयोग करके डेटा का विश्लेषण करने के नए तरीकों की खोज करेंगे। क्लस्टरिंग विशेष रूप से तब उपयोगी होती है जब आपके डेटासेट में लेबल की कमी होती है।  यदि इसमें लेबल हैं, तो वर्गीकरण (Classification) तकनीकें जैसे कि आपने पिछले पाठों में सीखी हैं, अधिक उपयोगी हो सकती हैं। लेकिन ऐसे मामलों में जहां आप बिना लेबल वाले डेटा को समूहबद्ध करना चाहते हैं, क्लस्टरिंग पैटर्न खोजने का एक शानदार तरीका है।

> उपयोगी निम्न-कोड (low code) उपकरण हैं जो क्लस्टरिंग मॉडल के साथ काम करने के बारे में सीखने में आपकी सहायता कर सकते हैं। इसके लिए [अझूरे ऍम एल (Azure ML)](https://docs.microsoft.com/learn/modules/create-clustering-model-azure-machine-learning-designer/?WT.mc_id=academic-77952-leestott) का प्रयोग करे 

## पाठ

1. [क्लस्टरिंग का परिचय](../1-Visualize/README.md)
2. [के-मीन्स क्लस्टरिंग](../2-K-Means/README.md)

## क्रेडिट

इन पाठों को [जेन लूपर](https://www.twitter.com/jenlooper) ने, [रीशित डागली](https://rishit_dagli) और [मुहम्मद साकिब खान इणां](https://twitter.com/Sakibinan) के सहयता से लिखा है। 

[नाइजीरियन संगीत](https://www.kaggle.com/sootersaalu/nigerian-songs-spotify) जो स्पॉटीफी से स्कैरेपेड है, उसे को कग्गले से लिया गया है। 

इस पाठ को बनाने में सहायता करने वाले उदाहरणों में यह के-मीन पाठ शामिल है: [आईरिस एक्सप्लोरेशन](https://www.kaggle.com/bburns/iris-exploration-pca-k-means-and-gmm-clustering), यह [परिचयात्मक नोटबुक](https://www.kaggle.com/prashant111/k-means-clustering-with-python), और यह [काल्पनिक एनजीओ उदाहरण](https://www.kaggle.com/ankandash/pca-k-means-clustering-hierarchical-clustering)। 
