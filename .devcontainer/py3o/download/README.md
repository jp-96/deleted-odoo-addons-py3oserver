# Source Code

## docker/py3o

filename: py3o-branch-default.zip
url: https://orus.io/xcg/docker/py3o



pip -y uninstall py3o.fusion

pip install \
	--upgrade \
	--find-links=https://wheelhouse.xcg.io/18.04/py3o.renderserver/ \
	--find-links=https://wheelhouse.xcg.io/18.04/py3o.fusion/ \
	py3o.fusion

