FROM pytorch/pytorch:latest

WORKDIR /

ENV RAW_DATA_FN '/tmp/urban-dictionary-words-dataset.zip'
ENV WASHED_DATA_FN '/tmp/jina/urbandict/urbandict-word-defs.csv'

COPY . /

RUN mv urban-dictionary-words-dataset.zip ${RAW_DATA_FN}

RUN python -m pip install --no-cache-dir --upgrade pip && \
    pip install --no-cache-dir -r requirements_docker.txt

RUN python -c "from transformers import DistilBertModel, DistilBertTokenizer; x='distilbert-base-cased'; DistilBertModel.from_pretrained(x); DistilBertTokenizer.from_pretrained(x)"

RUN python prepare_data.py && python app.py -t index -n 10000

RUN rm -f ${RAW_DATA_FN} ${WASHED_DATA_FN}

ENTRYPOINT ["python", "app.py","-t"]
