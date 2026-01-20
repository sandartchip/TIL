
- BAM, SAM 형태의 파일을 읽고 쓰고 조작할 수 있게 해 준다.

# Sam 파일

- SAM 파일은 Sequence alignment data를 담고 있는 텍스트 파일(.txt)로 각 내용들은 탭(tab)으로 분리되어 alignment, mapping 정보를 담고 있다. 
- Next-generation sequencing (NGS)을 통해 시퀀싱된 서열의 전사체 혹은 유전체 서열(Reference, @로 시작되는)에 FASTQ 파일을 다시 mapping시킨 형태의 파일이다.

- SAM 파일은 텍스트 파일의 문자열 형식으로 저장하여 바로 열람이 가능하며, 이를 압축하고 색인화하여 바이너리 형식으로 변환한 것이 BAM 파일이다.

- NGS의 발달로 불특정 다수의 organism에서 유전체 혹은 전사체 서열이 대량으로 시퀀싱되고 있다. 
- Human의 경우는 개인차에 의한 다수의 변이 정보를 밝히고 이것이 질병과 연관된 변이인지를 밝히기 위해 시퀀싱된 reads는 유전체서열에 다시 remapping되기도 하고, 새로운 organism의 유전체 정보를 밝히기 위해서도 remapping이 이뤄지고 있다. SAM 파일은 '1000 genome project'를 진행하면서 공동 연구의 효율성을 위해 데이터의 공유를 표준화 하려는 방안으로 채택된 remapping의 표준 포맷이다.

- Remapping을 위해 많이 이용하는 software인 BWA, Bowtie, CLCAssemblyCell등은 모두 mapping output으로 SAM 파일을 형성한다.

- SAM 파일들을 다루는데 필요한 software package로는 SAMtools가 있다.


# Bam 파일 

- FASTQ 데이터는 read라고 불리는 매우 짧은 서열(50~200bp)들로 구성된 파일임. 
- 보통 한 사람의 DNA를 NGS기기에 넣으면 개별 read의 개수는 백만 개가 넘으며, 경우에 따라선 1억 개가 넘기도 함. 
- FASTQ는 DNA 조각들(read)의 염기서열 정보를 가지고 있지만, 어떤 염색체 어느 위치에 있는 DNA인지에 대한 정보는 가지고있지 않음.

 



https://bio-info.tistory.com/26
