FROM langchain/langgraph-api:3.11

# Image metadata
LABEL name="claime-agent"
LABEL version="1.0.0"
LABEL description="Claime Multi-Agent Fact Checking System"

# -- Adding local package . --
ADD . /deps/agent
# -- End of local package . --

# -- Installing all local dependencies --
RUN PYTHONDONTWRITEBYTECODE=1 pip install --no-cache-dir -c /api/constraints.txt -e /deps/*
# -- End of local dependencies install --
ENV LANGSERVE_GRAPHS='{"claim_extractor": "/deps/agent/claim_extractor/agent.py:graph", "claim_verifier": "/deps/agent/claim_verifier/agent.py:graph", "fact_checker": "/deps/agent/fact_checker/agent.py:graph"}'

# -- Ensure user deps didn't inadvertently overwrite langgraph-api
RUN mkdir -p /api/langgraph_api /api/langgraph_runtime /api/langgraph_license && touch /api/langgraph_api/__init__.py /api/langgraph_runtime/__init__.py /api/langgraph_license/__init__.py
RUN PYTHONDONTWRITEBYTECODE=1 pip install --no-cache-dir --no-deps -e /api
# -- End of ensuring user deps didn't inadvertently overwrite langgraph-api --
# -- Removing pip from the final image ~<:===~~~ --
RUN pip uninstall -y pip setuptools wheel && rm -rf /usr/local/lib/python*/site-packages/pip* /usr/local/lib/python*/site-packages/setuptools* /usr/local/lib/python*/site-packages/wheel* && find /usr/local/bin -name "pip*" -delete
# -- End of pip removal --

WORKDIR /deps/agent
