# Datasheet for BitFinex SOLUSD/SOLUSDT/SOLF0:USTF0 project data
<br/><br/>


## Motivation

* #### For what purpose was the dataset created?
  The dataset was created for the purpose of investigating crypto security market microstructure and evaluating machine learning models for their performance in predicting short-term price returns.
  
    
* #### Who created the dataset (e.g., which team, research group) and on behalf of which entity (e.g., company, institution, organization)? Who funded the creation of the dataset?
  The dataset was created by the project author, using raw network packet data recorded from the Bitfinex published market data feed. The creation was self-funded by the author. The transformed data is not freely licensed: without other explicit, recorded permission, it may only be used for the purposes of evaluating this project by evaluators for the Imperial AI+ML Professional Certificate undertaken by the author.

  The dataset will not be maintained. Timeliness concerns in HFT situations dictate that the value of this dataset will decay substantially and rapidly.
<br/><br/>
 
## Composition

* #### What do the instances that comprise the dataset represent (e.g., documents, photos, people, countries)?
  The instances in this dataset represent time sampled change events in the orderbooks of a subset of crypto securities, specifically SOLUSD, and the time-related state of SOLUSDT and the SOLUSDT perpetual future, or CFD, trading as SOLF0USTF0.
  
* #### How many instances of each type are there?
  - 27,695 sampled update events for SOLUSD (USD cash)
  
	
* #### Is there any missing data?
  During daily file rotations, a subset of data representing approximately 10 minutes of a trading day is separately recorded and not used here. The data represents the balance of the trading day for the above mentioned securities for the days selected.
  
	
* #### Does the dataset contain data that might be considered confidential (e.g., data that is protected by legal privilege or by    doctor–patient confidentiality, data that includes the content of individuals’ non-public communications)?
  As the data is derived from "public" data feeds from the cryptocurrency exchange Bitfinex, there is no reason to believe any of the data could be subject to privacy concerns.  
<br/><br/>

## Collection process

* #### How was the data acquired?
  Raw market data network packets were recorded down. An order book handler processed the packets and maintained an order book. At each order book update event, a snapshot of the top ten levels of the book was emitted, which formed the base of the data set. 
  
	
* #### If the data is a sample of a larger subset, what was the sampling strategy? 
  The sampling strategy was to select 5 recent trading days on an underlying instrument that was sufficiently liquid to justify exploring, namely Solana (SOL), and its primary markets.
  
	
* #### Over what time frame was the data collected?
  The timestamps within the dataset disclose the times over which the data was collected, with the earliest timestamp being **2024-10-05 04:01:15.962069** and the latest timestamp being **2024-10-10 03:51:56.716628**.
<br/><br/>    
    
## Preprocessing/cleaning/labelling

* #### Was any preprocessing/cleaning/labeling of the data done (e.g., discretization or bucketing, tokenization, part-of-speech tagging, SIFT feature extraction, removal of instances, processing of missing values)? If so, please provide a description. If not, you may skip the remaining questions in this section. 

  The data was preprocessed into a normalised order book form, generating outputs in the shape:<br/>
  
* **msg_type**, **msg_size**, **filler**,   * **timestamp_saved** - not relevant for this project<br/>
* **timestamp** - the timestamp of the event causing the emission of the current state snapshot<br/>
* **sym_id** - the numerical symbol id uniquely identifying the relevant order book for which the snapshot is being generated<br/>
* **imb[1..10]** - the imbalance in total order quantity at the first...nth levels of the order book, computed as log(bid qty/ask qty)<br/>
* **cumimb[1..10]** - the imbalance in total order quantity at the first...nth levels of the order book inclusively, i.e. cumimb2 is computed as log((level 1 bid qty + level 2 bid qty)/(level 1 ask qty + level 2 ask qty))<br/>
* **ret_[b|f][various]** - the signed difference between the price (b[n] milliseconds before) or (f[n] milliseconds after) the current timestamp, computer as log(other time midprice / midprice now)
<br/><br/>
 
## Uses

* #### What other tasks could the dataset be used for?
  This dataset could be used for:
    - exploring crypto market microstructure; and 
    - searching for evidence of market manipulation (spoofing/momentum ignition/...)
    
	
* #### Is there anything about the composition of the dataset or the way it was collected and preprocessed/cleaned/labeled that might impact future uses? For example, is there anything that a dataset consumer might need to know to avoid uses that could result in unfair treatment of individuals or groups (e.g., stereotyping, quality of service issues) or other risks or harms (e.g., legal risks, financial harms)? If so, please provide a description. Is there anything a dataset consumer could do to mitigate these risks or harms? 

  The data is presented in anonymised form with no identifiable information relating to the market participants whose interests are represented in the public order book. There is no obvious concern around stereotyping or quality of service issues.
  
  The data should not be used to design trading strategies that explicitly exploit in an improper manner the behaviour of other market participants: this is a requirement that applies to participants in markets, regardless of the dataset.



* #### Are there tasks for which the dataset should not be used? If so, please provide a description.
  The dataset should not be used for purposes that could harm individuals or result in legal or financial risks.
<br/><br/>    

## Distribution

* #### How has the dataset already been distributed?
  It has not been distributed.
	
* #### Is it subject to any copyright or other intellectual property (IP) license, and/or under applicable terms of use (ToU)?  
  Yes - the transformed data is subject to copyright by the author of the project, and permission is granted to evaluators of the project to make use of the project and dataset purely for evaluation purposes in respect of the author's participation in the Imperial AI+ML Professional Certificate. 
<br/><br/>    

## Maintenance

* #### Who maintains the dataset?
  The dataset is not actively maintained: HFT datasets have limited useful lifespans for proprietary trading purposes. The data pipeline to generate similar data over different periods is maintained by the project author.
